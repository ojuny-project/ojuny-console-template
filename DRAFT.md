# DRAFT - ojuny-console-template

Status: draft scope. Nothing is built. This file describes what the repository
is supposed to contain, not what it contains today.

## Purpose

The living-room console edition of the Ojuny template. It behaves like a
console first and a desktop second: the machine boots into Steam Big Picture
Mode by default, can log in automatically, and still exposes a full desktop
target for management and non-Steam use.

It targets a TV, a controller and a single local account. It is not a general
desktop template with Steam added.

## What the repository will contain

```text
ojuny-console-template/
  flake.nix                  dendritic entrypoint
  flake.lock
  modules/
    01-users/                console account and auto-login
    02-core/                 display, audio, input, updates
    03-desktop/              session selection and boot behavior
    05-applications/         Steam, emulation and streaming
    06-utility/              controller and remote management
    07-system/               console hardware, networking, power
    backend/                 session presets and generated assets
    patches/                 reviewed patch switch
  scripts/                   setup, generators, lifecycle
  docs/                      console UX, sessions, first boot
  tests/                     evaluation and VM checks
```

### Defaults

- The default session is the console gaming session, running Steam Big Picture
  Mode full-screen.
- The account logs in automatically to that session.
- The desktop session is a selectable target, not the default.
- A settings page controls the default session, auto-login and the switch
  between gaming and desktop.
- Steam is installed and signed in on first use. No Steam account is stored in
  the repository.
- The console UI is controller-first and readable from a couch.
- `nix-web-gui` is offered as an optional management service so the machine can
  be configured from a phone or another computer.
- `nix-settings-gui` remains the local desktop settings surface.

### Settings pages

- Console: default session, auto-login, power behavior, boot splash.
- Display: resolution, refresh rate, HDR, VRR, overscan and TV-safe margins.
- Audio: output device, HDMI or DisplayPort, channel layout and volume.
- Controllers: pairing, layout, dead zones and per-controller profiles.
- Games: Steam, Proton, emulation and streaming clients.
- Desktop: desktop session choice and desktop-specific settings.
- Network: Wi-Fi, Ethernet, firewall and remote management.
- System: hardware report, updates and recovery.
- Management: enable `nix-web-gui`, bind address, access token and firewall
  opening.

### Sessions

- A gaming session running Steam Big Picture under a gamescope-style compositor.
- A desktop session provided by the selected Ojuny desktop layer.
- A greeter path for choosing the session when auto-login is off.
- A first-boot path that creates the console account and starts Steam.
- A recovery path that boots the desktop or a terminal when the gaming session
  fails.

### Compatibility

- Satisfies the minimum template requirements defined by
  `ojuny-minimal-template`.
- Same generated hardware, host and user paths.
- Same setup prompts and installer injection.
- Same patch switch and settings annotation rules.
- The console desktop layer is a separate input, not copied into the template.

### Documentation

- The console user journey: power on, pair a controller, sign in, play.
- How to switch to the desktop and make it the default.
- How auto-login interacts with the account password and disk encryption.
- How to reach `nix-web-gui` and how to secure it.
- The supported display, audio and controller hardware.

### Tests

- The flake evaluates and the console session builds.
- A VM boots to the gaming session with auto-login enabled.
- The desktop target starts when selected.
- `nix-web-gui` starts and is reachable only as configured.
- The installer installs this template through the generic path.
- Disabling auto-login produces a working greeter.

## Limits and non-goals

- `x86_64-linux` only.
- Steam is required for the primary experience. No DRM circumvention.
- No game compatibility guarantee. Proton results vary by title.
- No cloud gaming account provisioning.
- No handheld-specific hardware support in the first scope.
- No multi-user console mode by default.
- No replacement for SteamOS update channels or Valve certification.
- No always-on remote management listener. `nix-web-gui` is opt-in.

## Open questions

- Does auto-login also unlock the keyring, or does Steam ask for a password?
- Is the desktop target the full Ojuny desktop, a minimal GNOME session, or a
  console-specific shell?
- Is gamescope the required compositor, or can the session run nested?
- Does the template support more than one local console account?
- What access model should `nix-web-gui` use when it is enabled?
- Should the console template ship emulation and streaming clients, or keep
  them as patches?

## Sources

- `nixos-configs/notes/Ojuny Deployment Methods.md`.
- `ojuny-template/docs/00-overview.md`, `01-architecture.md`,
  `03-host-setup.md`.
- `ojuny-docs/08-scope-and-non-goals.md`.
