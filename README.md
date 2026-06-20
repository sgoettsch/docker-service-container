# docker-service-container

Ready-to-use Docker images published to the GitHub Container Registry under
`ghcr.io/sgoettsch/`. Pull them and run them — no local toolchain install
required.

Two kinds of images are provided:

- **Tool images** — run a single code-quality tool pinned to a fixed version
  (PHP-CS-Fixer).
- **Claude Code sandboxes** — preconfigured language environments for running
  [Claude Code](https://claude.ai/code) in a container.

## Available images

### PHP-CS-Fixer

[PHP-CS-Fixer](https://github.com/PHP-CS-Fixer/PHP-CS-Fixer) with the
`php-cs-fixer` binary as the entrypoint. One image per PHP runtime — pick the
one matching the PHP version your project targets.

| Image | PHP runtime |
|-------|-------------|
| `ghcr.io/sgoettsch/php-cs-fixer-8.2` | 8.2 |
| `ghcr.io/sgoettsch/php-cs-fixer-8.3` | 8.3 |
| `ghcr.io/sgoettsch/php-cs-fixer-8.4` | 8.4 |
| `ghcr.io/sgoettsch/php-cs-fixer-8.5` | 8.5 |

These replace the unmaintained
[`cytopia/php-cs-fixer`](https://github.com/cytopia/docker-php-cs-fixer) images.

## Usage

Mount your project into `/app` (the image's working directory) and pass any
`php-cs-fixer` argument. Swap `8.4` for the PHP version you target.

```bash
# Show the version and the PHP it runs on
docker run --rm ghcr.io/sgoettsch/php-cs-fixer-8.4 --version

# Dry run: show what would change (with diff), without writing
docker run --rm -v "$PWD:/app" ghcr.io/sgoettsch/php-cs-fixer-8.4 fix --dry-run --diff .

# Apply fixes in place
docker run --rm -v "$PWD:/app" ghcr.io/sgoettsch/php-cs-fixer-8.4 fix .

# CI check: no writes, non-zero exit if anything needs fixing
docker run --rm -v "$PWD:/app" ghcr.io/sgoettsch/php-cs-fixer-8.4 fix --dry-run --diff --stop-on-violation .
```

Notes:

- A `.php-cs-fixer.dist.php` / `.php-cs-fixer.php` config in your project root is
  picked up automatically. Point at a specific file with
  `--config=.php-cs-fixer.dist.php`.
- **PowerShell:** use `${PWD}` instead of `$PWD` for the volume mount.
- If php-cs-fixer refuses to run on a newer/unsupported PHP runtime, set
  `PHP_CS_FIXER_IGNORE_ENV=1` (add `-e PHP_CS_FIXER_IGNORE_ENV=1`).

### GitLab CI example

```yaml
php-cs-fixer:
  image: ghcr.io/sgoettsch/php-cs-fixer-8.4
  script:
    - php-cs-fixer fix --dry-run --diff --stop-on-violation .
```

## Claude Code sandbox: Elixir

`ghcr.io/sgoettsch/claude-elixir-sandbox` — an Elixir/Erlang development
environment for running Claude Code in a container. It is built on
`docker/sandbox-templates:claude-code` and comes with Erlang 27.1, Elixir
1.17.3-otp-27 (via asdf), Hex, Rebar, Node.js, and the PostgreSQL client
pre-installed.

Mount your project into `/workspace` (the image's working directory) and start
an interactive shell:

```bash
docker run -it --rm -v "$PWD:/workspace" ghcr.io/sgoettsch/claude-elixir-sandbox bash
```

Inside the container the Elixir toolchain is on `PATH`, so the usual commands
work directly:

```bash
mix deps.get
mix test
iex -S mix
```
