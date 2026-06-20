# The Tavern

A Rust reimplementation of the Battle.net account and authentication service,
built for the digital preservation of retired World of Warcraft Classic
client builds that Blizzard no longer serves.

The Tavern targets the Classic re-release client lines — Vanilla, The Burning
Crusade, Wrath of the Lich King, and Cataclysm Classic — up to defined build
cutoffs (see [Supported Client Builds](#supported-client-builds)). It provides
the subset of the Battle.net service those builds depend on: OAuth
authentication, account management, and the issuance of authentication tokens
the game client consumes before reaching a realm server.

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

| Client line | Recreates | Build cutoff |
| --- | --- | --- |
| 1.13.x (2019 to 2021) | Vanilla, 2005 (v1.12) | 39692 |
| 1.14.x (2021 to 2023) | Vanilla, 2005 (v1.12) | 51535 |
| 2.5.x through 2.5.4 | The Burning Crusade, 2007 | 44833 |
| 3.4.x through 3.4.4 | Wrath of the Lich King, 2008 | 61581 |
| 4.4.x through 4.4.2 | Cataclysm, 2010 | 60895 |

Build numbers are the values reported by the client at its login screen and
catalogued at [warcraft.wiki.gg][public-builds].

## Preservation Scope

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

## Goals

The Tavern is intended to provide the account and authentication backend that
classic World of Warcraft clients talk to before reaching a realm server. In
scope:

- OAuth 2.0 / OpenID Connect provider endpoints matching the flows used by the
  retired game client versions and the Battle.net desktop app.
- Account management: registration, credentials, and account state.
- Authentication token issuance, including the session and realm-login tokens
  the client exchanges for access to a game realm.
- A persistence layer for accounts and credentials.

Out of scope:

- Game world, realm emulation, and gameplay systems. Those belong to a realm
  server, not an account service.
- Any client version Blizzard currently serves, or any build above the
  supported cutoffs. See [Preservation Scope](#preservation-scope).

## Relationship to Battle.net

The Tavern reimplements the account and authentication protocols that retired
World of Warcraft Classic client builds expect, so those abandoned clients
remain usable for preservation. It is built for interoperability with those clients
only, not for use against any Blizzard service currently in operation.

Blizzard's Battle.net service, the "Battle.net" name, and World of Warcraft
are trademarks of their respective owners; protocol and product names are used
here descriptively for interoperability, not as a claim of affiliation or
endorsement.

## Tech Stack

- Language: Rust (stable, edition 2024).
- Toolchain management: [mise][mise].
- Linting: `cargo clippy` and the configured markdownlint rules.

## Development

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

## License

Copyright the wowemulation-dev contributors.

Distributed under the [GNU Affero General Public License v3.0 only][agpl]
(AGPL-3.0-only). See [`LICENSE`](LICENSE) for the full text. A network-served
derivative of this software must offer its users the corresponding source under
the same license.

[org]: https://github.com/wowemulation-dev
[mise]: https://mise.jdx.dev/
[agpl]: https://www.gnu.org/licenses/agpl-3.0.html
[public-builds]: https://warcraft.wiki.gg/wiki/Public_client_builds
