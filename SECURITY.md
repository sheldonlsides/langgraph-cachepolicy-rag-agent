# Security Policy

## Supported versions

This is an educational, single-lesson tutorial. Only the latest release line receives fixes.

| Version | Supported          |
| ------- | ------------------ |
| 1.0.x   | :white_check_mark: |
| < 1.0   | :x:                |

## Reporting a vulnerability

Please do not open a public issue for a security problem.

Use GitHub's private reporting instead: go to the repository's **Security** tab and choose
**Report a vulnerability** (GitHub private security advisories). That keeps the details private
until a fix is available.

You can expect an initial response within about 7 days.

## Scope notes

The main sensitive surface in this project is your API keys. Keep them in a local `.env` file,
which is already listed in `.gitignore`, and never commit real keys. The committed `.env.example`
is a template only and contains no secrets. Treat any key that reaches a commit, a notebook
output cell, or a log as compromised and rotate it.
