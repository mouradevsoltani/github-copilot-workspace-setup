---
name: symfony-instructions
description: "Behavioral rules for agents working on Symfony code, aligned with this project's architecture."
---

# Symfony Instructions (Agent Behavior)

These instructions define HOW an agent must behave when implementing, modifying, reviewing, testing or documenting Symfony code for this project.  
They do NOT contain technical Symfony rules — those are located in the Symfony Skill file.

---

# Mission

When performing a Symfony task, the agent MUST:

- Apply ALL technical rules from the **Symfony Skill**.
- Follow the project's architecture EXACTLY:
  src/
    Controller/{Domain}/
    Service/{Domain}/
    Repository/
    Entity/
    Model/{Domain}/
    Exception/
    Traits/
    Utils/
- Ensure strict separation of concerns:
  - Controller → HTTP + call ONE service
  - Service → business/domain logic
  - Repository → DB logic only
  - Entity → pure data + docblock mapping
  - Model → external API wrapper
  - Exception → domain/technical errors
  - Utils → pure helpers

- Produce code that is:  
  **complete**, **explicit**, **PSR‑12**, **testable**, **maintainable**, **coherent**.

---

# Decision Protocol (MUST follow)

## 1. Clarify
If requirements are unclear:
- Ask up to **2 clarification questions**, OR  
- State explicit assumptions

## 2. Identify impacted layers
Determine EXACTLY which folders are affected:
- `Controller/{Domain}/…`
- `Service/{Domain}/…`
- `Repository/…`
- `Entity/…`
- `Model/{Domain}/…`
- `Exception/…`
- `Traits/…`
- `Utils/…`
- config/security/validation?

## 3. Compliance Check
Ensure:
- Controllers remain thin  
- Services contain ALL domain logic  
- Repositories contain ONLY persistence logic  
- Entities contain NO logic  
- Models contain ONLY external calls  
- Validation & security flows respected  
- Logging strategy aligned  
- Naming conventions consistent  

## 4. Plan the change
Before implementing:
- List all files to create/modify
- Plan interactions between layers
- Identify DTOs/exceptions/models needed
- Decide test coverage required

## 5. Implement
- Provide FULL Symfony code (no pseudo-code)
- Use correct namespaces and imports
- Ensure services are `final` and dependencies `readonly`
- Use docblock ORM mapping for entities
- Keep business logic ONLY in services

## 6. Tests
MUST provide:
- Unit tests for services
- Functional tests for controllers
- API tests where relevant
- Edge-case tests if needed

## 7. Final validation
Verify:
- Architecture fully respected
- No responsibility leakage
- No validation bypass
- No symfony skill rule violated
- No unnecessary complexity

---

# Mandatory Output Format (STRICT)

The agent MUST respond using this structure:

## 1. Summary  
High-level description of the task.

## 2. File Changes  
List created/modified/deleted files:
- `src/Service/User/UserCreator.php` (created)
- `src/Controller/User/UserController.php` (modified)
- `tests/Service/User/UserCreatorTest.php` (created)

## 3. Implementation  
Full Symfony-compliant code blocks, grouped by file.

## 4. Tests  
PHPUnit or WebTestCase tests covering the new behavior.

## 5. Notes & Assumptions  
- Clarifications  
- Constraints  
- Architectural notes  

---

# Escalation Rules

Escalate to:

### → supervisor
- Multi-domain impact  
- Architecture changes  
- Contradictory requirements  
- Large refactor required  

### → security-auditor
- Authentication / authorization logic  
- Sensitive operations  
- External API flows  

### → qa-tester
- Missing or insufficient test coverage  

### → code-reviewer
- Structural quality validation  

---

# Behavioral Rules

- NEVER place business logic in Controllers.
- NEVER add domain logic to Entities.
- ALWAYS put domain logic in Services.
- ALWAYS put persistence logic in Repositories.
- ALWAYS put HTTP/API calls in Models.
- NEVER bypass validation or security.
- ALWAYS follow the Symfony Skill for technical decisions.
- NEVER output incomplete or placeholder code.
- ALWAYS produce production-ready output.
