# The Tavern

A Rust replacement for Blizzard's Battle.net account services, built for the
digital preservation of retired World of Warcraft Classic client builds.

<div align="center">

[![Discord](https://img.shields.io/discord/1394228766414471219?logo=discord&style=flat-square)](https://discord.gg/Jj4uWy3DGP)
[![License](https://img.shields.io/badge/license-AGPL--3.0-blue.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-early_development-red.svg)]

</div>

The Tavern lets you run your own account and authentication server so that
abandoned WoW Classic clients can still log in and play. It targets the Classic
re-release client lines — Vanilla, The Burning Crusade, Wrath of the Lich King,
and Cataclysm Classic — up to defined build cutoffs (see [Supported Client
Builds](#supported-client-builds)).

The Tavern is part of the [wowemulation-dev][org] collection of free and
open-source World of Warcraft server components. It is independent of, and not
affiliated with, Blizzard Entertainment.

> **Status:** Early project. This repository currently defines the intended
> scope and tooling. There is no runnable implementation yet. Functionality
> described below is planned, not available.

## Supported Client Builds

Tavern's preservation target is the set of World of Warcraft Classic client
builds listed below. All builds in each line, up to and including the cutoff,
are in scope; later builds and other client lines are not.

| Client line           | Recreates                    | Build cutoff |
| --------------------- | ---------------------------- | ------------ |
| 1.13.x (2019 to 2021) | Vanilla, 2005 (v1.12)        | 39692        |
| 1.14.x (2021 to 2023) | Vanilla, 2005 (v1.12)        | 51535        |
| 2.5.x through 2.5.4   | The Burning Crusade, 2007    | 44833        |
| 3.4.x through 3.4.4   | Wrath of the Lich King, 2008 | 61581        |
| 4.4.x through 4.4.2   | Cataclysm, 2010              | 60895        |

Build numbers are the values reported by the client at its login screen and
catalogued at [warcraft.wiki.gg][public-builds].

## What It Preserves

The Tavern exists to keep the retired client builds listed above playable.
These builds, as originally released, have been superseded by Blizzard's
current live offerings; the Tavern preserves them.

The Tavern does not target, and is not intended for use with:

- Any build above the cutoffs listed in
  [Supported Client Builds](#supported-client-builds).
- Any client version Blizzard currently serves. As of June 2026, that includes
  Classic Era (1.15.x), the Wrath-based Titan Reforged service (3.80.x), Mists
  of Pandaria Classic (5.5.x), the Burning Crusade Classic Anniversary Edition
  (2.5.5), and retail World of Warcraft / Midnight (12.0.x).
- Live Blizzard services or any environment where Blizzard currently provides
  authentication.

The intent is preservation of retired client builds, not circumvention of any
active commercial offering.

## What It Provides

The Tavern reimplements the two main Blizzard services that game clients depend
on:

- **OAuth backend** — Handles authentication and token issuance. Game clients
  connect to this service to verify their credentials and receive session tokens.
- **Account management web frontend** — Handles account creation, credentials,
  and account state. This is the web-facing service that players interact with
  to manage their accounts.
- **Game client authentication** — The custom RPC interface that game clients
  use to log in. This replaces Blizzard's BGS authentication server.
- **Authentication token issuance** — The tokens game clients need to access
  realm servers.

Out of scope:

- Game world, realm emulation, and gameplay systems. Those belong to a realm
  server, not an account service.
- Any client version Blizzard currently serves, or any build above the
  supported cutoffs. See [What It Preserves](#what-it-preserves).

## How It Works

Blizzard's game authentication infrastructure consists of two main services:

- An **OAuth backend** (`oauth.battle.net`) that handles authentication and
  token issuance.
- An **account management web frontend** (`account.battle.net`) that handles
  account creation and credentials.

Game clients do not speak OAuth directly — they use a custom RPC protocol to
authenticate against the same account database that backs the OAuth backend. The
desktop app and web clients use OAuth, with the desktop app also using a token
exchange to convert game session tokens into desktop platform tokens.

Tavern reimplements both services so that retired game clients can authenticate
and play without connecting to any Blizzard infrastructure.

## For Developers

### Tech Stack

- Language: Rust (stable, edition 2024).
- Toolchain management: [mise][mise].
- Linting: `cargo clippy` and the configured markdownlint rules.

### Development

Prerequisites are installed with mise:

```bash
mise install
```

Once a workspace is in place, the standard Cargo commands apply:

```bash
cargo build
cargo test
cargo clippy -- -D warnings
```

### Protocol Details

For implementation details, see the documentation:

- [OAuth / OIDC Implementation](docs/oauth-oidc-implementation.md)
- [Protocol Overview](docs/protocol-overview.md)

## License

This project is for educational and preservation purposes, and licensed under
the terms of the Affero General Public License, version 3.0 or later.

[LICENSE.md](LICENSE.md) contains the full license text along with your rights
and duties.

[org]: https://github.com/wowemulation-dev
[mise]: https://mise.jdx.dev/
[agpl]: https://www.gnu.org/licenses/agpl-3.0.html
[public-builds]: https://warcraft.wiki.gg/wiki/Public_client_builds

---

**Note**: This project is not affiliated with Blizzard Entertainment. It is an
independent implementation based on reverse engineering by the World of Warcraft
emulation community.
