# Home Assistant Add-on: eufy-security-ws — T8426 / E30 Support

![Logo](https://raw.githubusercontent.com/bropat/hassio-eufy-security-ws/master/eufy-security-ws/logo.png)

![Supports aarch64 Architecture](https://img.shields.io/badge/aarch64-yes-green.svg)
![Supports amd64 Architecture](https://img.shields.io/badge/amd64-yes-green.svg)
![Supports armhf Architecture](https://img.shields.io/badge/armhf-yes-green.svg)
![Supports armv7 Architecture](https://img.shields.io/badge/armv7-yes-green.svg)
![Supports i386 Architecture](https://img.shields.io/badge/i386-yes-green.svg)

> [!NOTE]
> ## About this fork
>
> This is a custom Home Assistant add-on build of
> [bropat/eufy-security-ws](https://github.com/bropat/eufy-security-ws).
>
> This build uses
> [meckelangelo/eufy-security-client](https://github.com/meckelangelo/eufy-security-client),
> a fork of
> [bropat/eufy-security-client](https://github.com/bropat/eufy-security-client),
> containing additional support for the **Eufy Floodlight Camera E30 (T8426)**.
>
> The T8426 changes are based on the fix proposed upstream in
> [bropat/eufy-security-client PR #949](https://github.com/bropat/eufy-security-client/pull/949).

> [!CAUTION]
> # 🚨🚨🚨 LIBRARY DEPRECATION NOTICE 🚨🚨🚨
>
> ### ⚠️ Eufy is shutting down the legacy APIs this library is built on. ⚠️
>
> Eufy is migrating its ecosystem to the newer **Eufy Mega** platform
> (the "5-in-1" app covering Security / Clean / Lights / Care) and has
> already started removing access to the legacy APIs on which this
> library was originally built.
>
> Push notifications have been restored against the new v6
> (`eufy_mega`) backend as a short-term solution. Other functionality
> that still depends on legacy endpoints may stop working as Eufy
> continues the migration.
>
> Once the legacy API is fully shut down, functionality dependent on
> those APIs will no longer work.
>
> A new integration built around Eufy Mega is under development by the
> upstream projects.
>
> **Treat the current release as a temporary stopgap.**
>
> See the
> [upstream eufy-security-ws project](https://github.com/bropat/eufy-security-ws)
> for the latest status of this migration.

## About

This add-on runs
[eufy-security-ws](https://github.com/bropat/eufy-security-ws), a server
wrapper around
[eufy-security-client](https://github.com/bropat/eufy-security-client)
that exposes supported Eufy Security devices through a WebSocket API.

It bridges events and device controls so Eufy Security devices can be
integrated with Home Assistant and other smart-home infrastructure.

For Home Assistant, this WebSocket server can be used with the
[Eufy Security integration](https://github.com/fuatakgun/eufy_security).

## T8426 / Floodlight Camera E30 support

The standard `eufy-security-client` command handling does not currently
provide working manual floodlight control for the Eufy Floodlight Camera
E30 (T8426).

This add-on builds `eufy-security-ws` with the
[meckelangelo/eufy-security-client](https://github.com/meckelangelo/eufy-security-client)
fork, which includes the T8426 command-routing changes required for
manual floodlight control.

The implementation is based on the upstream work proposed by `qike-ms`:

- [bropat/eufy-security-client PR #949](https://github.com/bropat/eufy-security-client/pull/949)
- [T8426 fix commit](https://github.com/bropat/eufy-security-client/commit/9b5d7ba4babca9b68d75f2d9687d8b1d2ad8086d)

The patched client used by this add-on is available here:

- [meckelangelo/eufy-security-client](https://github.com/meckelangelo/eufy-security-client)

The changes are intended to provide T8426/E30 support until equivalent
functionality is incorporated into the upstream projects.

## Installation

Add this repository to the Home Assistant add-on store as a custom
repository and install the T8426/E30 version of `eufy-security-ws`.

Configure the add-on with your Eufy Security account credentials and
other desired options, then start the add-on.

The add-on exposes the `eufy-security-ws` WebSocket service used by the
Home Assistant Eufy Security integration.

## Node 24 and RSA_PKCS1_PADDING

The Eufy cloud handshake requires `RSA_PKCS1_PADDING`, which Node.js
restricted as part of the Marvin attack fix (CVE-2023-46809).

Earlier versions of `eufy-security-ws` worked around this by launching
Node with:

    --security-revert=CVE-2023-46809

That flag is no longer recognized by Node 24+ and causes Node to abort
with:

    Error: Attempt to revert an unknown CVE

Current `eufy-security-client` versions provide an embedded pure-JavaScript
PKCS#1 implementation, enabled by default with:

    enableEmbeddedPKCS1Support: true

Therefore, this add-on does not pass the obsolete Node security-revert
flag.

## Upstream documentation

For general `eufy-security-ws` usage, WebSocket API documentation, and
other information, see:

https://bropat.github.io/eufy-security-ws/

Upstream projects:

- [bropat/eufy-security-ws](https://github.com/bropat/eufy-security-ws)
- [bropat/eufy-security-client](https://github.com/bropat/eufy-security-client)
- [bropat/hassio-eufy-security-ws](https://github.com/bropat/hassio-eufy-security-ws)

General issues that can be reproduced without the T8426-specific changes
should be reported to the appropriate upstream project.

## Credits

`eufy-security-ws` and `eufy-security-client` are developed and
maintained by [bropat](https://github.com/bropat) and their contributors.

The T8426/E30 changes used by this fork are based on work contributed by
[qike-ms](https://github.com/qike-ms) to the upstream
`eufy-security-client` project.

The development of `eufy-security-ws` was inspired by
[zwave-js-server](https://github.com/zwave-js/zwave-js-server).

## Support upstream development

If you appreciate the upstream project and want to support its
development:

[Ko-fi](https://ko-fi.com/E1E332Q6Z)  
[PayPal](https://www.paypal.me/pbroetto)

## Disclaimer

This project is not affiliated with Anker or Eufy (Eufy Security).
