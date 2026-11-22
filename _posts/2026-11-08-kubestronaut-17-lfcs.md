---
title: "LFCS: The Operating System Under the Cluster"
author: vishalanarase
date: 2026-11-08 12:00:00 +0530
categories: [The Road to Golden Kubestronaut]
tags: [kubestronaut, lfcs]
render_with_liquid: false
keywords: 'lfcs, linux foundation certified system administrator, kubestronaut, linux'
image: assets/images/kubestronaut/title/lfcs.jpg
---

This is the last exam on [The Road to Golden Kubestronaut](/posts/kubestronaut-00-introduction). [CNPE](/posts/kubestronaut-16-cnpe) was the platform. **LFCS** is the Linux system that platform runs on.

The Linux Foundation Certified System Administrator exam is performance-based. For two hours you solve tasks on a command line. It is not a CNCF exam. Golden Kubestronaut requires it anyway. Processes, networking, storage, and permissions are the work.

## Where this exam sits

Every cluster you touched on this road was a set of Linux machines. kubelet is a service. A container is a process. A volume is a filesystem. An Ingress is a proxy. LFCS makes those sentences something you can fix at a shell, not only with kubectl.

The exam is distribution-independent. You no longer pick a distro in the checklist and receive a different paper. The tasks are the ones a sysadmin meets on the job.

## Exam snapshot

Domains are from the [Linux Foundation LFCS page](https://training.linuxfoundation.org/certification/linux-foundation-certified-sysadmin-lfcs/).

| | |
| --- | --- |
| Exam | Linux Foundation Certified System Administrator (LFCS) |
| Format | Online, proctored, performance-based |
| Duration | 2 hours |
| Validity | 2 years |
| Prerequisite | None |
| Price | $445 for the exam, including one retake |
| Time to schedule | 12 months from purchase |
| Included | Two Killer.sh simulator attempts. Each attempt stays open for 36 hours. |

Confirm the pass mark in the candidate handbook the week you book. Do not assume it matches CKA.

## Syllabus

### Operations Deployment, 25%

Tied for the largest domain.

- Kernel parameters, the ones that persist and the ones that do not.
- Processes and services. Find them, manage them, and know why one failed.
- Scheduled jobs.
- Packages and repositories. Install, verify, and keep them.
- Recovery when the disk, the OS, or the hardware path breaks.
- Virtual machines with libvirt.
- A container engine. Create a container and manage it. You have lived in Kubernetes. This is the layer underneath.
- SELinux, as mandatory access control you can read and enforce.

### Networking, 25%

The other large domain.

- IPv4, IPv6, and hostname resolution.
- Time sync.
- Troubleshooting a path that does not connect.
- OpenSSH, server and client.
- Packet filtering, port redirection, and NAT.
- Static routing.
- Bridges and bonds.
- A reverse proxy and a load balancer. You have configured these inside Kubernetes. Here you configure them on the host.

### Storage, 20%

- LVM.
- The virtual filesystem.
- Create a filesystem and repair one.
- Remote filesystems and network block devices.
- Swap.
- Automounters.
- Storage performance, enough to know which disk is hurting.

### Essential Commands, 20%

- Everyday Git.
- Service units. Create one, start one, and read why it failed.
- Performance of the system and of a service.
- What an application needs from the OS: ports, files, users.
- A full disk.
- TLS certificates. Create them, place them, and notice when they are wrong.

### Users and Groups, 10%

The smallest domain, and the one that locks you out when you get it wrong.

- Local users and groups.
- Profiles, personal and system-wide.
- Resource limits.
- ACLs on files.
- LDAP for accounts that are not local.

## How to prepare

1. Read the competencies on the [LFCS page](https://training.linuxfoundation.org/certification/linux-foundation-certified-sysadmin-lfcs/).
2. Use a Linux VM you are allowed to break. Add a user, a systemd service, an LVM volume, a firewall rule, and an SSH config. Reboot. Confirm they survived.
3. Practice with man pages. They are allowed during the exam, along with docs installed on the system. The [resources page](https://docs.linuxfoundation.org/tc-docs/certification/certification-resources-allowed) says what is in bounds. A personal cheatsheet is not.
4. Use both Killer.sh attempts.

## Resources

1. **The exam page.** [Linux Foundation LFCS](https://training.linuxfoundation.org/certification/linux-foundation-certified-sysadmin-lfcs/). This exam is not in the CNCF catalog.
2. **What you may read during the exam.** Man pages and the distribution docs under `/usr/share`.
3. **The simulator.** Two Killer.sh attempts, included with registration.
4. **The course sold with the exam.** Linux System Administration Essentials (LFS207) can be bundled on the exam page.

## Before you book

One monitor, a private room, a matching photo ID, a clear desk, and a computer that can install the secure browser. Plug the laptop in. You have two hours.

Then go sit it. This is the last exam on the road.

The next post is **[Keeping the title](/posts/kubestronaut-18-golden)**. It is not another exam. It is how you stay a Golden Kubestronaut after the last sitting.
