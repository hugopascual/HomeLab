# Contributing

## Commits

Commits follow [Conventional Commits](https://www.conventionalcommits.org/):

```text
<type>(<optional scope>): <description>

<optional body>

<optional footer>
```

- Types: `feat`, `fix`, `refactor`, `docs`, `chore`.
- Scope: the service or area changed (`dns`, `vpn`, `backup`, `makefile`...).
- Description in English, lowercase, imperative mood and without final
  period.
- A commit can group several changes, each one in its own
  `<type>: <description>` line.
- Changes that require updating existing `.env` files or change `make`
  targets are marked with `!` and a `BREAKING CHANGE:` footer.

Examples:

```text
feat(vpn): make the wireguard interface name configurable

fix(backup): skip tools that are not deployed

feat(dns)!: move the web interface port to the services env file

BREAKING CHANGE: add PIHOLE_WEB_INTERFACE_PORT to services/dns/.env
```

Dependabot image updates use the `chore` type (see [updates](updates.md)).
