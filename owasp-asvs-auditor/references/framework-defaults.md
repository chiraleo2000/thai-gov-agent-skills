# Framework Secure Defaults

Reference for Decision Tree Step 2 — Framework defaults that satisfy ASVS requirements
out of the box. If a bypass is found, the default no longer applies (continue to Step 3).

---

## Django (Python)

| ASVS Area | Default Behaviour | Bypass Patterns |
| --------- | ----------------- | --------------- |
| XSS / Output Encoding (V1) | Template auto-escaping ON by default | `{% autoescape off %}`, `mark_safe(`, `SafeString(` |
| CSRF (V3) | `CsrfViewMiddleware` enabled by default | `@csrf_exempt`, removing middleware from `MIDDLEWARE` |
| SQL Injection (V1) | ORM uses parameterised queries | `raw()`, `extra()`, `RawSQL()`, `.execute()` with string formatting |
| Clickjacking (V3) | `X-Frame-Options: DENY` via `XFrameOptionsMiddleware` | `@xframe_options_exempt`, removing middleware |
| Session Security (V7) | `SESSION_COOKIE_HTTPONLY = True` by default | `SESSION_COOKIE_HTTPONLY = False` |
| Password Hashing (V6) | PBKDF2 with SHA-256 (default hasher) | Custom hashers using MD5/SHA1 |
| HSTS (V12) | Available via `SECURE_HSTS_SECONDS` (not on by default) | Must be explicitly configured |

---

## Express.js (Node.js)

| ASVS Area | Default Behaviour | Bypass Patterns |
| --------- | ----------------- | --------------- |
| XSS / Output Encoding (V1) | None by default — requires template engine configuration | N/A (no default) |
| CSRF (V3) | None by default — requires `csurf` or similar | N/A |
| SQL Injection (V1) | Depends on ORM (Prisma/Sequelize use parameterised queries) | Raw queries with string concatenation |
| Security Headers (V3) | None by default — use `helmet` middleware | Not installing helmet |
| Session Security (V7) | `express-session` defaults: `httpOnly: true`, but `secure: false` | `httpOnly: false`, missing `secure: true` |
| JSON Body Parsing (V4) | `express.json()` validates Content-Type | `type: '*/*'` override |

---

## Next.js (React)

| ASVS Area | Default Behaviour | Bypass Patterns |
| --------- | ----------------- | --------------- |
| XSS / Output Encoding (V1) | React auto-escapes JSX by default | `dangerouslySetInnerHTML` |
| CSRF (V3) | API routes don't have built-in CSRF | N/A |
| Security Headers (V3) | Configurable via `next.config.js` headers | Missing headers configuration |
| Image Optimisation (V5) | `next/image` validates domains | `remotePatterns: [{ hostname: '*' }]` |

---

## Spring Boot (Java)

| ASVS Area | Default Behaviour | Bypass Patterns |
| --------- | ----------------- | --------------- |
| CSRF (V3) | Spring Security enables CSRF by default | `csrf().disable()`, `@EnableWebSecurity` with custom config |
| XSS / Output Encoding (V1) | Thymeleaf auto-escapes by default | `th:utext` (unescaped), `@{...}` in certain contexts |
| SQL Injection (V1) | JPA/Hibernate use parameterised queries | `createNativeQuery()` with string concatenation |
| Authentication (V6) | Spring Security provides full auth framework | `@NoSecurity`, `permitAll()` on sensitive endpoints |
| Session (V7) | Server-side session with secure defaults | `server.servlet.session.cookie.http-only=false` |
| CORS (V4) | Blocked by default | `@CrossOrigin(origins = "*")` |
| Password Encoding (V6) | BCryptPasswordEncoder recommended | `NoOpPasswordEncoder`, `MD5PasswordEncoder` |

---

## Flask (Python)

| ASVS Area | Default Behaviour | Bypass Patterns |
| --------- | ----------------- | --------------- |
| XSS / Output Encoding (V1) | Jinja2 auto-escaping ON for `.html` templates | `{% autoescape false %}`, `Markup()`, `\|safe` filter |
| CSRF (V3) | None by default — requires Flask-WTF | N/A |
| SQL Injection (V1) | SQLAlchemy ORM uses parameterised queries | `db.engine.execute()` with string formatting |
| Session (V7) | Client-side signed cookies (not encrypted) | Weak `SECRET_KEY`, no server-side session |

---

## FastAPI (Python)

| ASVS Area | Default Behaviour | Bypass Patterns |
| --------- | ----------------- | --------------- |
| Input Validation (V2) | Pydantic model validation on all request bodies | `Any` type hints, skipping validation |
| SQL Injection (V1) | SQLAlchemy/Tortoise ORM parameterised queries | Raw SQL with f-strings |
| CORS (V4) | Blocked by default | `allow_origins=["*"]` with credentials |
| OpenAPI Docs (V13) | Swagger UI exposed by default | Leaving `/docs` and `/redoc` in production |

---

## Laravel (PHP)

| ASVS Area | Default Behaviour | Bypass Patterns |
| --------- | ----------------- | --------------- |
| XSS / Output Encoding (V1) | Blade `{{ }}` auto-escapes | `{!! !!}` (raw output) |
| CSRF (V3) | `VerifyCsrfToken` middleware enabled | `$except` array, removing middleware |
| SQL Injection (V1) | Eloquent ORM uses parameterised queries | `DB::raw()`, `whereRaw()` with user input |
| Password Hashing (V6) | Bcrypt by default | Custom hasher with weak algorithm |
| Session (V7) | Server-side, secure cookie defaults | `secure => false` in production |

---

## NestJS (Node.js)

| ASVS Area | Default Behaviour | Bypass Patterns |
| --------- | ----------------- | --------------- |
| Input Validation (V2) | `ValidationPipe` with class-validator (opt-in) | Not enabling global validation pipe |
| CORS (V4) | Disabled by default | `app.enableCors({ origin: '*' })` |
| Helmet Headers (V3) | Available via `@nestjs/helmet` | Not installing/configuring |
| Authentication (V6) | Passport integration available | `@Public()` decorator bypassing guards |

---

## Ruby on Rails

| ASVS Area | Default Behaviour | Bypass Patterns |
| --------- | ----------------- | --------------- |
| XSS / Output Encoding (V1) | ERB auto-escapes by default | `raw()`, `html_safe`, `<%== %>` |
| CSRF (V3) | `protect_from_forgery` enabled | `skip_before_action :verify_authenticity_token` |
| SQL Injection (V1) | ActiveRecord uses parameterised queries | `.where("name = '#{params[:name]}'")`, `.find_by_sql` with interpolation |
| Mass Assignment (V8) | Strong Parameters required | `permit!` (permit all) |
| Session (V7) | Server-side session, encrypted cookies | Weak `secret_key_base` |
