---
name: symfony-security
description: Symfony security best practices for authentication, authorization, validation, CSRF, secure uploads, secrets, rate limiting, and production hardening.
---

# Symfony Security Best Practices

Comprehensive security guidance for Symfony applications to protect against common vulnerabilities.

## When to Activate

- Adding authentication or authorization
- Handling user input and file uploads
- Building new API endpoints
- Managing secrets and environment settings
- Hardening production deployments

## How It Works

- Security firewalls and access_control define baseline protection per route space.
- Authenticators and user providers handle login and identity lifecycle.
- Voters enforce authorization in controllers and services.
- Symfony Validator secures data boundaries in DTOs, forms, and request mapping.
- Rate limiting via Symfony RateLimiter protects login, reset, and write endpoints.
- Data safety comes from explicit DTO mapping, signed URLs, and encrypted secrets management.

## Core Security Settings

- `APP_DEBUG=false` in production
- `APP_ENV=prod` in production
- `APP_SECRET` must be strong and rotated on compromise
- Configure secure cookie flags and SameSite policy
- Configure trusted proxies for correct HTTPS detection (`framework.trusted_proxies`)

## Session and Cookie Hardening

- Use `cookie_secure: auto` (or always in HTTPS-only environments)
- Set `cookie_httponly: true` and strict SameSite for sensitive flows
- Regenerate session IDs on login and privilege changes

## Authentication and Tokens

- Use Symfony Security authenticators for web and API flows
- Prefer short-lived tokens (JWT/OAuth2) for sensitive APIs
- Revoke or rotate tokens/credentials on logout, reset, and compromise

Example route protection:

```php
declare(strict_types=1);

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\JsonResponse;
use Symfony\Component\Routing\Attribute\Route;

final class MeController extends AbstractController
{
    #[Route('/api/me', methods: ['GET'])]
    public function __invoke(): JsonResponse
    {
        $this->denyAccessUnlessGranted('IS_AUTHENTICATED_FULLY');

        return $this->json(['id' => $this->getUser()?->getUserIdentifier()]);
    }
}
```

## Password Security

- Hash passwords with `UserPasswordHasherInterface`, never store plaintext
- Use secure reset-token workflows with expiration and one-time usage

```php
use Symfony\Component\PasswordHasher\Hasher\UserPasswordHasherInterface;

$hashed = $passwordHasher->hashPassword($user, $plainPassword);
$user->setPassword($hashed);
```

## Authorization: Voters and Access Rules

- Use voters for domain-level authorization decisions
- Enforce authorization in controllers and services
- Never perform authorization checks inline in service logic — delegate to voters

Example voter:

```php
declare(strict_types=1);

use Symfony\Component\Security\Core\Authentication\Token\TokenInterface;
use Symfony\Component\Security\Core\Authorization\Voter\Voter;

final class ProjectVoter extends Voter
{
    public const UPDATE = 'PROJECT_UPDATE';

    protected function supports(string $attribute, mixed $subject): bool
    {
        return $attribute === self::UPDATE && $subject instanceof Project;
    }

    protected function voteOnAttribute(string $attribute, mixed $subject, TokenInterface $token): bool
    {
        $user = $token->getUser();

        if (!$user instanceof User) {
            return false;
        }

        /** @var Project $project */
        $project = $subject;

        return $project->getOwner() === $user;
    }
}
```

Invoke from a controller:

```php
$this->denyAccessUnlessGranted('PROJECT_UPDATE', $project);
```

Use access control for route-level enforcement:

```yaml
# config/packages/security.yaml
security:
  access_control:
    - { path: ^/api/admin, roles: ROLE_ADMIN }
    - { path: ^/api, roles: IS_AUTHENTICATED_FULLY }
```

## Validation and Data Sanitization

- Always validate inputs with DTOs, Forms, or Validator constraints
- Use strict validation rules and type checks
- Never trust request payloads for derived fields

- Example DTO constraints:

```php
use Symfony\Component\Validator\Constraints as Assert;

final class UpdateProfileInput
{
    #[Assert\NotBlank]
    #[Assert\Length(max: 120)]
    public string $displayName;

    #[Assert\Email]
    public string $email;
}
```

## SQL Injection Prevention

- Use Doctrine QueryBuilder and parameter binding
- Avoid string-concatenated SQL and DQL

```php
$qb = $entityManager->createQueryBuilder();
$qb->select('u')
   ->from(User::class, 'u')
   ->where('u.email = :email')
   ->setParameter('email', $email);
```

## XSS Prevention

- Twig escapes output by default (`{{ value }}`)
- Use `|raw` only for trusted, sanitized HTML
- Sanitize rich text with a dedicated library

## CSRF Protection

- Keep CSRF protection enabled for state-changing browser requests
- Include and validate CSRF tokens in Symfony forms
- For API tokens (stateless APIs), do not rely on CSRF as a primary control

Example Twig form token:

```twig
<input type="hidden" name="_token" value="{{ csrf_token('upload_invoice') }}">
```

## File Upload Safety

- Validate file size, MIME type, and extension
- Store uploads outside public web root when possible
- Scan files for malware if required
- Generate server-side filenames, never trust client-provided names

```php
use Symfony\Component\HttpFoundation\File\UploadedFile;
use Symfony\Component\HttpKernel\Exception\UnprocessableEntityHttpException;

if ($invoiceFile instanceof UploadedFile) {
    if ($invoiceFile->getMimeType() !== 'application/pdf') {
        throw new UnprocessableEntityHttpException('Invalid file type.');
    }

    $safeName = bin2hex(random_bytes(16)).'.pdf';
    $invoiceFile->move($privateUploadPath, $safeName);
}
```

## Rate Limiting

- Apply rate limits on login, reset, and write-heavy endpoints
- Use IP + identity keys where possible
- Declare limiters in `config/packages/rate_limiter.yaml`

Configuration:

```yaml
# config/packages/rate_limiter.yaml
framework:
  rate_limiter:
    login:
      policy: sliding_window
      limit: 5
      interval: '1 minute'
```

Example limiter:

```php
use Symfony\Component\HttpKernel\Exception\TooManyRequestsHttpException;
use Symfony\Component\RateLimiter\RateLimiterFactory;

public function login(string $email, string $ip, RateLimiterFactory $loginLimiter): void
{
    $limiter = $loginLimiter->create(strtolower($email).'|'.$ip);
    $limit = $limiter->consume(1);

    if (!$limit->isAccepted()) {
        throw new TooManyRequestsHttpException(
            $limit->getRetryAfter()->getTimestamp() - time(),
            'Too many attempts. Please wait before retrying.'
        );
    }
}
```

## Secrets and Credentials

- Never commit secrets to source control
- Use environment variables and Symfony Secrets
- Rotate keys after exposure and invalidate sessions

## Encryption at Rest

Use explicit cryptography for highly sensitive payloads at rest (for example via libsodium/OpenSSL wrappers), with key management outside source code.

Do not hardcode cryptographic keys in code.

## Security Headers

- Add CSP, HSTS, and frame protection where appropriate
- Enforce HTTPS and trusted proxy configuration

Example subscriber to set headers:

```php
use Symfony\Component\EventDispatcher\EventSubscriberInterface;
use Symfony\Component\HttpKernel\Event\ResponseEvent;
use Symfony\Component\HttpKernel\KernelEvents;

final class SecurityHeadersSubscriber implements EventSubscriberInterface
{
    public static function getSubscribedEvents(): array
    {
        return [KernelEvents::RESPONSE => 'onResponse'];
    }

    public function onResponse(ResponseEvent $event): void
    {
        $response = $event->getResponse();

        $response->headers->add([
            'Content-Security-Policy' => "default-src 'self'",
            'Strict-Transport-Security' => 'max-age=31536000',
            'X-Frame-Options' => 'DENY',
            'X-Content-Type-Options' => 'nosniff',
            'Referrer-Policy' => 'no-referrer',
        ]);
    }
}
```

## CORS and API Exposure

- Restrict origins in NelmioCorsBundle or custom CORS config
- Avoid wildcard origins for authenticated routes

```yaml
# config/packages/nelmio_cors.yaml
nelmio_cors:
  defaults:
    allow_credentials: true
    allow_origin: ['https://app.example.com']
    allow_headers: ['Content-Type', 'Authorization']
    allow_methods: ['GET', 'POST', 'PUT', 'PATCH', 'DELETE']
  paths:
    '^/api/': ~
```

## Logging and PII

- Never log passwords, tokens, or full card data
- Redact sensitive fields in structured logs

```php
use Psr\Log\LoggerInterface;

/* @var LoggerInterface $logger */
$logger->info('User updated profile', [
    'user_id' => $user->getId(),
    'email' => '[REDACTED]',
    'token' => '[REDACTED]',
]);
```

## Dependency Security

- Run `composer audit` regularly
- Pin dependencies with care and update promptly on CVEs

## Signed URLs

Use signed routes for temporary, tamper-proof links.

```php
use Symfony\Component\Routing\Generator\UrlGeneratorInterface;
use Symfony\Component\HttpKernel\UriSigner;

$url = $urlGenerator->generate('downloads.invoice', ['invoice' => $invoiceId], UrlGeneratorInterface::ABSOLUTE_URL);
$signedUrl = $uriSigner->sign($url);
```

```php
if (!$uriSigner->check($request->getUri())) {
    throw $this->createAccessDeniedException('Invalid signature.');
}
```

## Security Checklist

- Deny-by-default access model for protected resources
- Voters used for resource-level authorization
- Input validation at all entry points
- Query parameterization for DB access
- CSRF enabled for browser forms and state-changing requests
- Strict upload validation and private storage for sensitive files
- Rate limiting on auth and write endpoints
- Secure headers and restrictive CORS policy in production
- Secrets managed outside source code
- Logs redacted for PII and credentials

## Testing and Verification

- Functional security tests for unauthenticated and unauthorized access
- Voter unit tests for authorization decisions
- Integration tests for upload, CSRF, and rate limit behavior
- Run `composer audit` and SAST checks in CI

```php
// Example: access denied assertion in functional test
$client->request('GET', '/api/admin/users');
self::assertResponseStatusCodeSame(401);
```

## Notes

- Keep business logic in Services; Controllers stay thin and security-aware.
- Never bypass validation or authorization in background handlers or internal endpoints.