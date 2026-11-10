# OpenWrt x86-64 images

This repository builds two separate OpenWrt 25.12.6 UEFI/EXT4 images for the B75M-A router:

- **Forkop** with the upstream `sing-box-forkop` **FULL** package.
- **Tachyon** with the upstream Rust `tachyon-core` package.

Both workflows are started manually from **Actions**:

- **Build OpenWrt Forkop image**
- **Build OpenWrt Tachyon image**

Each run uses the latest stable upstream release of the selected project, builds its OpenWrt packages against the 25.12.6 x86/64 SDK, then includes those packages in the official OpenWrt ImageBuilder output. The ImageBuilder archive is checked against the published SHA-256 digest. The firmware and its checksum are uploaded as a workflow artifact and retained for 30 days; the workflow does not publish a GitHub Release.

## Network defaults

The first-boot defaults are based on the supplied LuCI screenshot:

- `eth0` / `br-lan`: static `10.10.0.2/24`, connected toward RB5009 at `10.10.0.1`.
- `eth1` / `wan`: DHCP from the ONT (`192.168.1.0/24`).
- Static route to `10.10.10.0/24` via `10.10.0.1`.
- DHCP and IPv6 router advertisements disabled on OpenWrt.
- Argon LuCI theme and Russian LuCI language packages included.

RB5009 remains responsible for client DHCP and needs its own default route through `10.10.0.2`, plus the required DNS/firewall policy for AdGuard Home at `10.10.10.119`. These workflows do not change RB5009.

No VPN credentials, private keys, subscription URLs, or provider secrets are stored in this repository or embedded in the images. Add and configure those on the router after installation.

## Hardware notes

The two detected NICs report PCI vendor/device IDs `10ec:8168` (Realtek RTL8168/RTL8111 family). The workflows request `kmod-r8169` and `r8169-firmware` from the pinned OpenWrt release. No 2.5GbE adapter was identified in the supplied hardware output.

## Privacy

This repository is currently public. The workflow files and generated artifacts may therefore be visible to anyone with access to the repository. The image contains the private network addresses above, but no credentials.
