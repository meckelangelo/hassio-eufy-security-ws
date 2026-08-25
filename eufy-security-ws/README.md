# eufy-security-ws — T8426 / E30 Home Assistant Build

> [!NOTE]
> ## About this fork
>
> This Home Assistant add-on is a custom build of
> [bropat/eufy-security-ws](https://github.com/bropat/eufy-security-ws).
>
> It uses a custom fork of
> [eufy-security-client](https://github.com/meckelangelo/eufy-security-client)
> containing additional support for the **Eufy Floodlight Camera E30 (T8426)**.
>
> The T8426 changes are based on the fix proposed in
> [bropat/eufy-security-client#949](https://github.com/bropat/eufy-security-client/pull/949).
>
> This fork is intended primarily to provide T8426/E30 support until the
> corresponding changes are available through the upstream projects.

> [!CAUTION]
> # 🚨🚨🚨 LIBRARY DEPRECATION NOTICE 🚨🚨🚨
>
> ### ⚠️ Eufy is shutting down the legacy APIs this library is built on. ⚠️
>
> Eufy is in the middle of a large migration of their ecosystem. The newer **Eufy Mega**
> platform (the "5-in-1" app, covering Security / Clean / Lights / Care) is gradually
> becoming the only supported backend, and Eufy has **already started removing access to
> the legacy APIs** this library was originally built on. Until recently both worked in
> parallel — that is no longer guaranteed.
>
> **🔔 What this means for you:**
>
> - 🟢 A recent PR restores **push notifications** against the new v6 ("eufy_mega")
>   backend, so push works again **for now**. This is a short-term stopgap.
> - 🟡 Other functionality that still depends on legacy endpoints may stop working
>   **without warning** as Eufy continues the rollout. The current Eufy app no longer
>   uses the legacy API at all.
> - 🔴 Once the legacy API is fully shut down, **this library will stop functioning** —
>   no amount of patching here will change that.
>
> **🚧 What's next:**
>
> A new integration built around **Eufy Mega** is in active development (auto-discovery,
> less battery drain for P2P), designed from the ground up rather than bolted onto the
> Security-only structure, and coordinated across the Home Assistant, Homebridge and
> Homey communities so the new library works for everyone.
>
> ### 👉 Treat this release as a **temporary stopgap.** 👈
>
> *This notice originates from the upstream eufy-security-ws project and may be updated
> there as the migration progresses.*

## About

This add-on runs [eufy-security-ws](https://github.com/bropat/eufy-security-ws),
a small server wrapper around
[eufy-security-client](https://github.com/bropat/eufy-security-client)
that exposes Eufy Security devices through a WebSocket API.

This build uses the
[meckelangelo/eufy-security-client](https://github.com/meckelangelo/eufy-security-client)
fork instead of the standard `eufy-security-client` package in order to provide
additional support for the **Eufy Floodlight Camera E30 (T8426)**.

It can be used with the
[Home Assistant Eufy Security integration](https://github.com/fuatakgun/eufy_security)
to expose supported Eufy devices and controls to Home Assistant.

## T8426 / Floodlight Camera E30 support

The custom `eufy-security-client` build includes changes to recognize the T8426
and use the appropriate command path for features that differ from earlier
Eufy floodlight camera models.

In particular, this addresses manual floodlight control on the T8426/E30.

The implementation is based on work proposed upstream in:

- [bropat/eufy-security-client PR #949](https://github.com/bropat/eufy-security-client/pull/949)
- [T8426 fix commit](https://github.com/bropat/eufy-security-client/pull/949/commits/9b5d7ba4babca9b68d75f2d9687d8b1d2ad8086d)

The patched client used by this add-on can be found at:

- [meckelangelo/eufy-security-client](https://github.com/meckelangelo/eufy-security-client)

## Upstream projects

This project depends heavily on the work of the upstream Eufy Security projects:

- [bropat/eufy-security-ws](https://github.com/bropat/eufy-security-ws)
- [bropat/eufy-security-client](https://github.com/bropat/eufy-security-client)
- [bropat/hassio-eufy-security-ws](https://github.com/bropat/hassio-eufy-security-ws)

Please report general `eufy-security-ws` or `eufy-security-client` issues to the
appropriate upstream project when they are reproducible without the T8426-specific
changes in this fork.

## `RSA_PKCS1_PADDING` / CVE-2023-46809

The Eufy cloud handshake requires `RSA_PKCS1_PADDING`, which Node.js restricted
as part of the Marvin attack fix (CVE-2023-46809).

Earlier versions worked around this by launching Node with
`--security-revert=CVE-2023-46809`, but that flag is no longer recognized on
Node 24+ and causes Node to abort on startup with:

    Error: Attempt to revert an unknown CVE

This is now handled by `eufy-security-client`'s embedded pure-JavaScript PKCS#1
implementation, which is enabled by default with:

    enableEmbeddedPKCS1Support: true

No Node security-revert flag is required.

## Documentation

For general `eufy-security-ws` documentation, API information, and project
documentation, see:

https://bropat.github.io/eufy-security-ws/

## Credits

`eufy-security-ws` and `eufy-security-client` are developed and maintained by
[bropat](https://github.com/bropat) and their contributors.

The T8426/E30 support used by this fork is based on work by
[qike-ms](https://github.com/qike-ms) submitted to the upstream
`eufy-security-client` project.

The development of `eufy-security-ws` was inspired by
[zwave-js-server](https://github.com/zwave-js/zwave-js-server).

## Disclaimer

This project is not affiliated with Anker or Eufy (Eufy Security).# Home Assistant Add-on: eufy-security-ws

![Logo][logo]

[![Release][release-shield]][release] ![Project Maintenance][maintenance-shield]

![Supports aarch64 Architecture][aarch64-shield] [![Docker aarch64 Pulls][docker-aarch64-shield]][docker-aarch64]

![Supports amd64 Architecture][amd64-shield] [![Docker amd64 Pulls][docker-amd64-shield]][docker-amd64]

![Supports armhf Architecture][armhf-shield] [![Docker armhf Pulls][docker-armhf-shield]][docker-armhf]

![Supports armv7 Architecture][armv7-shield] [![Docker armv7 Pulls][docker-armv7-shield]][docker-armv7]

![Supports i386 Architecture][i386-shield] [![Docker i386 Pulls][docker-i386-shield]][docker-i386]

Allows you to use your Eufy devices.

It bridges events and allows you to control your Eufy devices via websocket. In this way you can integrate your Eufy devices with whatever smart home infrastructure you are using.

See Documentation tab for more details.

[logo]: https://raw.githubusercontent.com/bropat/hassio-eufy-security-ws/master/eufy-security-ws/logo.png
[docker-amd64-shield]: https://img.shields.io/docker/pulls/bropat/hassio-eufy-security-ws-amd64?label=docker%20pulls%20amd64&logo=docker
[docker-amd64]: https://hub.docker.com/repository/docker/bropat/hassio-eufy-security-ws-amd64/general
[docker-aarch64-shield]: https://img.shields.io/docker/pulls/bropat/hassio-eufy-security-ws-aarch64?label=docker%20pulls%20aarch64&logo=docker
[docker-aarch64]: https://hub.docker.com/repository/docker/bropat/hassio-eufy-security-ws-aarch64/general
[docker-armhf-shield]: https://img.shields.io/docker/pulls/bropat/hassio-eufy-security-ws-armhf?label=docker%20pulls%20armhf&logo=docker
[docker-armhf]: https://hub.docker.com/repository/docker/bropat/hassio-eufy-security-ws-armhf/general
[docker-armv7-shield]: https://img.shields.io/docker/pulls/bropat/hassio-eufy-security-ws-armv7?label=docker%20pulls%20armv7&logo=docker
[docker-armv7]: https://hub.docker.com/repository/docker/bropat/hassio-eufy-security-ws-armv7/general
[docker-i386-shield]: https://img.shields.io/docker/pulls/bropat/hassio-eufy-security-ws-i386?label=docker%20pulls%20i386&logo=docker
[docker-i386]: https://hub.docker.com/repository/docker/bropat/hassio-eufy-security-ws-i386/general
[aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg
[armhf-shield]: https://img.shields.io/badge/armhf-yes-green.svg
[armv7-shield]: https://img.shields.io/badge/armv7-yes-green.svg
[i386-shield]: https://img.shields.io/badge/i386-yes-green.svg
[maintenance-shield]: https://img.shields.io/maintenance/yes/2024.svg
[release-shield]: https://img.shields.io/badge/version-v1.9.3-blue.svg
[release]: https://github.com/bropat/eufy-security-ws/releases/tag/1.9.3
Join us on Discord:

<a target="_blank" href="https://discord.gg/5wjQ2asb64"><img src="https://dcbadge.limes.pink/api/server/5wjQ2asb64" alt="" /></a>
