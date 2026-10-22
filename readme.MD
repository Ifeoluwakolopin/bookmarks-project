# Bookmarks
A simple social website for bookmarking images found across different sites.<br>
Bookmarks can be viewed by other users and liked.
Users can also follow each other.


## Frameworks Used
- Django (Python)

## Runtime and dependencies

Use Python 3.13 (validated with 3.13.17) and Pipenv. From `bookmarks/`:

```sh
pipenv --python 3.13
pipenv sync
pipenv verify
pipenv run python -m pip check
pipenv run python manage.py check
```

`Pipfile.lock` is generated with Pipenv 2026.10.2. Django stays on the
supported 5.2 LTS line, currently 5.2.18. Pillow 13.0.0 addresses advisories
that also affect 12.3.0. Social-auth-app-django 6.0.1 permits the patched
social-auth-core 5.0.2; core also requires Requests 2.34 or newer.
Redis-py 8.1.0 replaces the old client that imports Python's removed
`distutils` module. The existing Redis commands and database selection remain.

The Django upgrade preserves integer primary keys with `AutoField`, replaces
the removed `is_ajax()` API with the same `X-Requested-With` header check,
and uses a CSRF-protected POST form at the existing logout URL.
Apply the normal Django and dependency migrations to a backed-up development
database before running the application; no application migrations were changed.

Settings are development defaults, not a production deployment configuration.
Use deployment-specific secrets, hosts and debug settings. Local environment
loading can be disabled with `PYTHON_DOTENV_DISABLED=1`; no real credentials
are needed for isolated SQLite checks.

The upgrade was checked with isolated SQLite migrations, username/email login,
AJAX likes/follows and pagination, POST logout/CSRF, JPEG thumbnails, OAuth
backend/URL imports, and an RS256 signing round-trip. Live Redis and provider
OAuth round-trips require separately configured infrastructure and were not run.
