# Logging and Rotation (12-Month Retention)

## Goal
Retain application logs for 12 months and purge older archives automatically.

## Baseline
- Use Monolog for app logs.
- Write logs under `/var/log/<app>/`.
- Rotate logs monthly with `logrotate`.
- Keep `rotate 12` archives.
- Never log secrets, tokens, or sensitive personal data.

## Monolog Example
```yaml
# config/packages/monolog.yaml
monolog:
  handlers:
    app:
      type: stream
      path: '/var/log/myapp/app.log'
      level: info
```

## logrotate Example
```conf
# /etc/logrotate.d/myapp
/var/log/myapp/*.log {
  monthly
  rotate 12
  missingok
  notifempty
  compress
  delaycompress
  dateext
  create 0640 www-data www-data
  sharedscripts
  postrotate
    /usr/bin/systemctl reload php8.2-fpm > /dev/null 2>&1 || true
  endscript
}
```

## Validation
- `logrotate -d /etc/logrotate.d/myapp` is valid.
- New logs are writable by runtime user.
- Rotation keeps at most 12 monthly archives.
