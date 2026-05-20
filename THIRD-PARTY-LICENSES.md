# Third-Party Licenses

Action Archive for BigFix Platform incorporates and depends on open-source
software developed by third parties. This file lists those components and
their license terms, separately from the project's own [LICENSE](LICENSE)
file. Nothing in the project LICENSE supersedes or alters the terms of any
third-party license listed below.

## What is bundled vs. what is fetched

Two distinct kinds of third-party code are involved:

1. **Bundled in the distribution.** Code that ships inside the release
   tarball, including compiled binary form. The only third-party software
   that ships this way is the **Cython** runtime support code, which is
   embedded by the Cython compiler into the project's compiled `.so` files.
2. **Fetched by the installer.** The Python packages listed in
   `requirements.txt` are not included in the release tarball. They are
   downloaded from PyPI during installation by `pip install -r
   requirements.txt`, and each arrives on the installed system with its own
   `LICENSE` and `METADATA` files intact. They are listed here for
   transparency and to document the project's full attribution chain.

System services such as PostgreSQL and Nginx are installed by `apt` from the
operating system's own repositories and are not redistributed by this
project. They are listed at the end of this file for completeness.

---

## Embedded in the distribution

### Cython

- **Purpose:** Source-to-C compiler. The project's Python source modules
  are compiled to native shared libraries (`.so`) at build time, and the
  resulting binaries embed portions of Cython's runtime support code.
- **License:** Apache License 2.0 (SPDX: `Apache-2.0`)
- **Copyright:** © Cython Development Team
- **Upstream:** https://cython.org/
- **License text:** https://www.apache.org/licenses/LICENSE-2.0

A short-form `NOTICE` follows, satisfying Apache License 2.0 Section 4(d):

> This product includes software developed by the Cython project
> (https://cython.org/).
> Copyright © Cython Development Team. Licensed under the Apache License,
> Version 2.0.

---

## Runtime dependencies (fetched by pip from PyPI at install time)

The following packages are installed by the project's installer via
`pip install -r requirements.txt`. They are not bundled in the release
tarball. Each package arrives from PyPI with its own `LICENSE` and
`METADATA` file, which is the canonical record of its terms at the version
fetched.

| Package | Minimum version | SPDX License | Copyright holders (summary) | Upstream |
|---|---|---|---|---|
| fastapi | 0.115.0 | MIT | © Sebastián Ramírez | https://github.com/tiangolo/fastapi |
| uvicorn | 0.30.0 | BSD-3-Clause | © Encode OSS Ltd. | https://github.com/encode/uvicorn |
| python-multipart | 0.0.9 | Apache-2.0 | © Andrew Dunham | https://github.com/Kludex/python-multipart |
| starlette | 0.40.0 | BSD-3-Clause | © Encode OSS Ltd. | https://github.com/encode/starlette |
| SQLAlchemy | 2.0.30 | MIT | © SQLAlchemy authors and contributors | https://www.sqlalchemy.org/ |
| psycopg2-binary | 2.9.9 | LGPL-3.0-or-later with OpenSSL exception, also available under the PostgreSQL License | © The Psycopg Team | https://www.psycopg.org/ |
| alembic | 1.13.0 | MIT | © Mike Bayer | https://alembic.sqlalchemy.org/ |
| PyJWT | 2.8.0 | MIT | © José Padilla | https://github.com/jpadilla/pyjwt |
| bcrypt | 4.1.0 | Apache-2.0 | © The Python Cryptographic Authority | https://github.com/pyca/bcrypt |
| passlib | 1.7.4 | BSD-3-Clause | © Assurance Technologies, LLC and contributors | https://passlib.readthedocs.io/ |
| cryptography | 42.0.0 | Apache-2.0 OR BSD-3-Clause | © Individual contributors | https://cryptography.io/ |
| httpx | 0.27.0 | BSD-3-Clause | © Encode OSS Ltd. | https://www.python-httpx.org/ |
| lxml | 5.0.0 | BSD-3-Clause (with components under BSD-3-Clause, MIT, and ZPL-2.0) | © Infrae and contributors | https://lxml.de/ |
| APScheduler | 3.10.0 | MIT | © Alex Grönholm | https://github.com/agronholm/apscheduler |
| tzlocal | 5.0 | MIT | © Lennart Regebro and contributors | https://github.com/regebro/tzlocal |
| pydantic | 2.5.0 | MIT | © Pydantic Services Inc. | https://docs.pydantic.dev/ |
| pydantic-settings | 2.1.0 | MIT | © Pydantic Services Inc. | https://github.com/pydantic/pydantic-settings |
| python-dotenv | 1.0.0 | BSD-3-Clause | © Saurabh Kumar and contributors | https://github.com/theskumar/python-dotenv |
| psutil | 5.9.0 | BSD-3-Clause | © Jay Loden, Dave Daeschler, Giampaolo Rodola | https://github.com/giampaolo/psutil |
| reportlab | 4.0.0 | BSD-3-Clause | © ReportLab Inc. | https://www.reportlab.com/opensource/ |
| pillow | 10.0.0 | HPND (PIL-style permissive) | © 1995-2011 Fredrik Lundh and contributors; current fork © 2010-present Jeffrey A. Clark (Alex) and contributors | https://python-pillow.org/ |
| python-dateutil | 2.8.0 | Apache-2.0 OR BSD-3-Clause | © Gustavo Niemeyer and contributors | https://github.com/dateutil/dateutil |
| pytz | 2024.1 | MIT | © Stuart Bishop | https://pythonhosted.org/pytz/ |

For each package the canonical license text is the `LICENSE` file shipped
inside the package on PyPI at the version actually installed. The SPDX
identifiers above are provided so that automated license-scanning tools can
classify each component. Where a package offers a choice between licenses
(`Apache-2.0 OR BSD-3-Clause`), the user may rely on either at their option.

### A note on psycopg2-binary

`psycopg2-binary` is the only dependency with a copyleft-leaning license
(LGPL-3.0). Its terms are satisfied in this project because Python's
`import` mechanism is dynamic linking, and users remain free to replace the
installed `psycopg2-binary` with any compatible build of their choice. The
package is additionally made available by its authors under the PostgreSQL
License when used to access PostgreSQL, which removes copyleft
considerations entirely for this use case.

---

## Operating system services

The following components are installed by the project's installer through
the operating system's own package manager (`apt`). They are not bundled in
the release tarball and are not redistributed by this project. They are
listed here so that adopters have a complete picture of the licenses that
apply to a deployed system.

- **Python interpreter.** PSF License 2.0. © Python Software Foundation.
  Installed via `apt install python3`.
- **PostgreSQL.** PostgreSQL License (BSD-style permissive).
  © The PostgreSQL Global Development Group.
  Installed via `apt install postgresql`.
- **Nginx.** BSD-2-Clause. © Igor Sysoev and Nginx, Inc.
  Installed via `apt install nginx`.
- **Ubuntu Server.** The base operating system is not part of this
  distribution.

---

## Bundled audit agent

The release tarball includes `agent/action-archive-audit-agent-latest.zip`,
which is the project's own Windows-side companion agent. Its code is
published under the same project [LICENSE](LICENSE). If a future release of
the audit agent adds third-party dependencies of its own, those will be
documented in this file.

---

## How to update this file

Update this file in tandem with `requirements.txt`. When a dependency is
added, removed, or has its license change, edit the corresponding row above
and reference the new SPDX identifier. When a new third-party component
becomes bundled in the distribution (rather than fetched at install time),
add it to the **Embedded in the distribution** section above, since that is
the section that carries the strongest attribution obligations.
