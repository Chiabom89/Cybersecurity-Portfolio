# Linux-Security
## Lab Scenario: Host Hardening via Linux Capabilities

### 1. The Challenge: Over-Privileged Root Binaries
In production IoT environments, background telemetry services (such as our mock `vax_monitor`) often require low-level access to raw network sockets. Standard configurations frequently run these services natively as the `root` user, creating an extreme security risk: if the binary is compromised via an exploit, the attacker instantly inherits full root execution rights across the entire host operating system.

### 2. The Solution: Implementing the Principle of Least Privilege
Instead of assigning a blanket `root` execution profile, we isolated the binary and used granular Linux Capabilities (`libcap2-bin`) to provide only the exact kernel permissions required.

- **Target Binary Deployed:** `/home/codespace/firmware_lab/vax_monitor`
- **Assigned Capability Set:** `CAP_NET_RAW`
- **Execution Vector Flags:** `+ep` (Effective and Permitted)

### 3. Technical Verification & Security Audit Trail
By stripping all standard administrative privileges and running a `getcap` verification audit, we proved the application is confined to a minimal permissions footprint:

```bash
$ getcap /home/codespace/firmware_lab/vax_monitor
/home/codespace/firmware_lab/vax_monitor cap_net_raw=ep
