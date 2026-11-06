# Secure Self-Hosted File Server (TrueNAS SCALE + Tailscale)

A private, Dropbox-style file server I built and run for a small remote team (3 users in Florida, Tennessee and New Jersey). Files stay on hardware I control, there are no upload or download size caps, and nothing is exposed to the public internet.

**Status:** Phase 1 complete and in active use. Phase 2 in progress.

---

## Goals

- Shared storage for 3 remote users, with no file-size limits
- No open ports on the home router (zero public exposure)
- Users can reach only the file server, nothing else on my network
- Data survives a single drive failure
- Simple enough for non-technical users to connect on Mac, Windows and iPhone

## Architecture

```
 Remote users (TN, NJ)                 Home network (FL)
 ┌───────────────────┐                 ┌──────────────────────────────┐
 │ Mac / PC / iPhone │                 │  AT&T gateway                │
 │  + Tailscale app  │                 │  (DHCP reservation, no port  │
 └─────────┬─────────┘                 │   forwarding)                │
           │  Encrypted WireGuard       │        │                     │
           │  tunnel (Tailscale)        │  TrueNAS SCALE server        │
           └──────────────────────────► │   • Tailscale app            │
                                        │   • SMB share: /shared       │
                                        │   • ZFS RAIDZ1 pool (4 HDD)  │
                                        └──────────────────────────────┘
```

## Hardware

| Component | Detail |
| --- | --- |
| Boot drive | ADATA SU630 SSD (TrueNAS OS only) |
| Data drives | 4 × 1 TB HDD |
| Pool layout | ZFS RAIDZ1, 1 vdev, ~2.7 TB usable |
| Network | Onboard Ethernet, wired to AT&T gateway |

## What I built (Phase 1)

1. **Installed TrueNAS SCALE** on a dedicated SSD so the OS is separate from the data.
2. **Gave the server a fixed address** with a DHCP reservation on the AT&T gateway, so the IP never changes.
3. **Created a RAIDZ1 storage pool** across 4 drives, so the data survives one drive failure.
4. **Created one dataset (`shared`)** with the SMB preset for the team's files.
5. **Set up role-based access:** a `team` group, one account per user, and an ACL that gives only that group Full Control of the share.
6. **Enabled the SMB service** and tested access from Windows, macOS and iOS on the local network.
7. **Installed Tailscale on TrueNAS** for remote access, with key expiry disabled on the server node.
8. **Shared only the server node** with each remote user, instead of inviting them to my whole tailnet.
9. **Wrote a one-page connection guide** for end users (Mac, Windows, iPhone, with a QR code shortcut). A sanitized copy is in [`docs/user-connection-guide.md`](docs/user-connection-guide.md).

## Security decisions

| Decision | Why it matters |
| --- | --- |
| No port forwarding; remote access only through Tailscale (WireGuard) | The server has zero attack surface on the public internet. |
| Node sharing instead of full tailnet invites | Least privilege: remote users can reach the file server only, not other devices at my home. |
| Group-based ACL on the dataset | Access is managed in one place; adding or removing a user is just a group change. |
| Individual accounts per user | Every action can be tied to a person, and access can be revoked one user at a time. |
| Separate credentials for the file server, Tailscale and Apple/Microsoft accounts | One leaked password doesn't unlock everything. |
| Passwords sent separately from the connection guide | The guide can be shared without exposing credentials. |
| RAIDZ1 with ZFS checksums | Protects against a single drive failure and silent data corruption (availability and integrity). |
| OS on its own SSD | An OS reinstall doesn't touch user data. |

## Problems I hit and fixed

- **iPhone couldn't connect.** iOS needs `smb://<server>` (not the Windows `\\server\share` format), and the share is picked after you log in. I documented the right format for each platform in the user guide.
- **Choosing a static IP method.** I used a DHCP reservation on the router instead of a static IP in TrueNAS. That avoids IP conflicts and the risk of locking myself out of the web UI.

## Roadmap

- [x] Phase 1: TrueNAS install, RAIDZ1 pool, SMB share, group ACLs, Tailscale remote access, user guide
- [ ] Phase 2: Browser-based file access (FileBrowser or Nextcloud) over Tailscale
- [ ] Scheduled ZFS snapshots (protection against accidental deletes and ransomware)
- [ ] Off-site / cloud backup of the `shared` dataset (3-2-1 rule)
- [ ] Email alerts for drive health (SMART) and pool status
- [ ] Tailscale ACL policy file to restrict users to the SMB port only
- [ ] Turn on 2FA for the TrueNAS admin account

## Skills shown

Network storage (ZFS, RAID), Linux-based NAS administration, SMB file sharing, access control (users, groups, ACLs), zero-trust remote access (Tailscale/WireGuard), network configuration (DHCP reservations), least privilege, troubleshooting across Windows/macOS/iOS, and end-user documentation.

## Screenshots

See the [`screenshots`](screenshots/) folder. IP addresses, usernames and other identifying details are blurred.
