# tidal-player-gnome-extension

A planned GNOME Shell extension for viewing and controlling TIDAL playback.

**Status:** planning and documentation only; the extension is not implemented yet.

## Goals

- Provide simple music controls in the GNOME panel.
- Explore OAuth login for catalogue search and other supported account features.
- Evaluate connecting to mobile or desktop applications through TIDAL Connect.

## Feasibility

Reviewed on November 19, 2026.

The public TIDAL API does not provide the TIDAL Connect integration needed to
discover and remotely control the official mobile or desktop applications.
OAuth login does not grant access to that functionality.

[TIDAL's Connect documentation](https://developer.tidal.com/documentation/connect)
states:

> We currently only support TIDAL Connect integrations for our device partners.

TIDAL Connect targets compatible speakers and audio hardware. It is not a public
API for arbitrary app-to-app playback control.

| Feature | Supported approach |
| --- | --- |
| OAuth login, catalogue search and metadata | Public TIDAL API, subject to available scopes and permissions |
| Current track and playback controls for a local Linux player | MPRIS over D-Bus, when supported by the player |
| Discover or control the official mobile/desktop apps through TIDAL Connect | No supported public integration |
| Transfer playback between those apps through TIDAL Connect | No supported public integration |
| Make Linux a TIDAL Connect receiver | Requires a device-partner integration; unavailable through the public API |
| Playback inside a custom application | Public SDK documentation currently describes third-party playback of previews |

## Recommended MVP

Use **MPRIS**, the standard Linux media-player control interface, to communicate
with an existing local TIDAL player. [TIDAL Hi-Fi](https://github.com/Mastermindzh/tidal-hifi)
is one candidate that supports MPRIS.

```text
GNOME Shell extension
        |
        | session D-Bus / MPRIS
        v
Local TIDAL player (for example, TIDAL Hi-Fi)
        |
        v
TIDAL playback
```

Planned controls:

- Display the current track, artist, album and available artwork.
- Play or pause playback.
- Skip to the previous or next track, when supported by the player.
- Show when no compatible player is running.

The player handles login and audio playback. The extension does not need its own
OAuth login for MPRIS controls. MPRIS communicates with a player in the local
desktop session; it does not provide remote control of a phone or another computer.

OAuth-backed catalogue features can be considered separately after the local
controls work. TIDAL Connect support would require additional access from TIDAL
and is not part of this MVP.

## References

- [TIDAL Connect developer documentation](https://developer.tidal.com/documentation/connect)
- [TIDAL API and SDK overview](https://developer.tidal.com/documentation/api-sdk/api-sdk-overview)
- [TIDAL Connect requirements and supported usage](https://support.tidal.com/hc/en-us/articles/360004565898-Tidal-Connect)
- [MPRIS specification](https://specifications.freedesktop.org/mpris-spec/latest/)
- [TIDAL Hi-Fi](https://github.com/Mastermindzh/tidal-hifi)
