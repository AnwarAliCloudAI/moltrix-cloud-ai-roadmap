# Phase 0 Host Capacity and Virtualization Decision

## Document Status

- **Version:** 0.1
- **Status:** Approved for initial implementation
- **Assessment date:** July 20, 2026
- **Program:** Moltrix Cloud & AI Transformation Program
- **Phase:** Phase 0 — Continuous Technical Foundation
- **Decision:** Use Microsoft Hyper-V as the Phase 0 virtualization platform

## Business and Learning Context

Moltrix requires a safe and repeatable environment for learning operating systems, networking, DNS, identity, permissions, automation, and troubleshooting.

The lab must allow Anwar to build, test, break, observe, and repair systems without placing the physical host or personal information at unnecessary risk.

Before selecting a virtualization platform, the proposed Windows host was assessed for processor capacity, memory, storage, operating-system support, hardware virtualization, security configuration, and installed virtualization features.

## Host Assessment

Only non-sensitive technical characteristics were collected. Computer names, serial numbers, product keys, usernames, and personal information were excluded.

| Area | Observed result | Assessment |
|---|---|---|
| Host operating system | Microsoft Windows 11 Pro, 64-bit | Suitable |
| Windows version | 25H2, build 26200 | Suitable; compatibility will be verified before software installation |
| Processor | AMD Ryzen 7 260 with Radeon 780M Graphics | Suitable |
| Processor capacity | 8 cores and 16 logical processors | Strong for the initial lab |
| Hardware virtualization | Enabled in firmware | Required condition satisfied |
| Usable memory | 15.3 GB | Suitable with controlled VM allocation |
| System-drive capacity | 901.8 GB | Suitable |
| Available system-drive space | 804.1 GB at assessment | Strong capacity for VMs and snapshots |
| Host architecture | 64-bit | Required condition satisfied |
| Virtualization-based security | Enabled and running | Security control preserved |
| Memory Integrity | Configured and running | Security control preserved |
| Initial Hyper-V state | Disabled | Available but not initially installed |
| VMware state | Not installed at assessment | No existing VMware environment to preserve |

## Capacity Conclusion

The Lenovo Windows 11 Pro host is capable of supporting the initial Moltrix Systems & Network Foundation Lab.

Processor and storage capacity are strong. Memory is the primary constraint.

The initial design can support:

- DC01 — Windows Server
- CLIENT01 — Windows 11
- LINUX01 — Linux

The three machines should not automatically receive large fixed-memory allocations. Dynamic memory and staged operation will be considered so the Windows host retains sufficient resources.

Final VM allocations will be decided after guest operating-system requirements are confirmed.

## Previous VMware Observation

A previous VMware environment had been removed before this assessment.

### Reported symptom

Virtual machines could not communicate with one another.

### Evidence retained

No configuration records, screenshots, IP information, logs, or test results were preserved before removal.

### Possible causes

Possible causes include:

- Virtual machines connected to different virtual networks
- Incorrect IP addresses or subnet masks
- Disabled or disconnected virtual network adapters
- Windows or guest firewall behaviour
- Incorrect routing or DNS assumptions

### Root-cause status

**Not confirmed.**

The earlier symptom must not be presented as a solved incident because the original environment can no longer be tested.

### Lesson carried forward

Future lab problems will be investigated layer by layer, and evidence will be collected before configurations or software are removed.

## Security Finding

The Windows host reported:

- Virtualization-based security enabled and running
- Memory Integrity configured and running

These controls use hardware virtualization to isolate sensitive Windows security operations.

Moltrix will not disable a host security control merely to simplify lab installation unless a future documented requirement, risk assessment, and approved decision justify the change.

Reference:

- [Microsoft — Enable memory integrity](https://learn.microsoft.com/windows/security/hardware-security/enable-virtualization-based-protection-of-code-integrity)

## Options Considered

### Option 1 — VMware Workstation

Modern VMware Workstation versions can operate on supported Windows hosts. When Windows virtualization-based security is active, VMware may operate through the Windows hypervisor platform rather than receiving direct access to hardware virtualization.

VMware remained technically possible, but it would add another compatibility and troubleshooting layer to this Microsoft-focused foundation lab.

Reference:

- [Broadcom — VMware Workstation behaviour with Windows VBS](https://knowledge.broadcom.com/external/article/417896/after-the-host-os-upgrade-to-version-win.html)

### Option 2 — Microsoft Hyper-V

Hyper-V is Microsoft’s native virtualization platform and is available on the Windows 11 Pro host.

It supports the required Windows Server, Windows client, Linux, virtual-switch, checkpoint, and network-isolation learning activities.

It also aligns directly with the Moltrix Microsoft endpoint, Azure, security, and platform-engineering journey.

Reference:

- [Microsoft — Hyper-V overview](https://learn.microsoft.com/windows-server/virtualization/hyper-v/overview)

### Option 3 — Disable VBS for VMware

This option could reduce some VMware compatibility or performance constraints, but it would weaken the host’s current security posture.

This option was rejected.

## Architecture Decision

Moltrix will use **Microsoft Hyper-V** as the initial Phase 0 virtualization platform.

The decision is based on:

- Native Windows 11 Pro integration
- Compatibility with the Microsoft-focused roadmap
- Preservation of VBS and Memory Integrity
- Support for the planned Windows Server, Windows 11, and Linux guests
- Support for virtual switches, isolation, checkpoints, and structured networking labs
- Removal of an unnecessary third-party virtualization layer
- Opportunity to learn a Microsoft virtualization technology relevant to later infrastructure work

This decision does not mean VMware is an unsuitable product. It means Hyper-V is the better fit for this host, security posture, and learning story.

## Implementation Result

Hyper-V was enabled using Windows optional-feature management.

After the required restart, the following conditions were verified:

| Verification | Result |
|---|---|
| `Microsoft-Hyper-V-All` | Enabled |
| Windows hypervisor present | True |
| Hyper-V Virtual Machine Management service (`vmms`) | Running |
| `vmms` start type | Automatic |
| VBS and Memory Integrity | Preserved |
| Virtual machines created | None yet |
| Virtual switches created | None yet |

The Hyper-V host installation is therefore complete.

## Risk Controls

Before creating virtual machines, Moltrix will:

- Define the virtual-network purpose
- Choose network isolation deliberately
- Create an IP-addressing plan
- Allocate host and guest memory conservatively
- Confirm guest licensing and evaluation rights
- Install one machine at a time
- Test each network adapter before adding services
- Test IP communication before DNS
- Test DNS before domain operations
- Preserve evidence before changing a failed configuration
- Sanitize all public screenshots and command output

## Immediate Next Steps

1. Update the Phase 0 topology diagram from VMware Workstation to Microsoft Hyper-V.
2. Update the Phase 0 blueprint to reflect this architecture decision.
3. Design the virtual-switch and IP-addressing plan.
4. Confirm approved operating-system sources and licensing.
5. Create the first virtual machine only after the design is documented.
6. Record successful and failed tests as Phase 0 evidence.

## Decision Summary

The host-capacity gate has been passed.

Hyper-V is installed and verified without disabling the host’s active security protections. Moltrix can now proceed to virtual-network design, but no virtual machine should be created until that design is documented and understood.