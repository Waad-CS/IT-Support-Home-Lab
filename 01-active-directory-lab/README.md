# Active Directory Lab

A hands-on lab covering Windows Server 2022, Active Directory, and everyday help desk tasks, built in VirtualBox on a laptop with 8 GB of RAM. All accounts and passwords are lab-only test data.

## Lab environment

| Component | Details |
|---|---|
| Hypervisor | Oracle VirtualBox 7.2 |
| DC01 | Windows Server 2022 Standard Evaluation (Desktop Experience), 2 GB RAM, 50 GB disk, domain controller |
| SRV01 | Windows Server 2022 Standard Evaluation (Desktop Experience), 2 GB RAM, 50 GB disk, domain member |
| Domain | `lab.local` |
| Network | VirtualBox Internal Network `labnet`, `192.168.10.0/24` (DC01 `.10`, SRV01 `.20`) |

## 1. Virtual machine and Windows Server installation

Created the `DC01` virtual machine and installed Windows Server 2022 with the Desktop Experience.

![VM name and ISO](screenshots/01-vm-name-and-iso.png)
![VM hardware](screenshots/02-vm-hardware.png)
![VM created](screenshots/03-vm-created.png)
![Select edition](screenshots/04-select-edition.png)
![Server desktop after first sign-in](screenshots/05-server-desktop.png)

## 2. Active Directory domain controller

Renamed the server to `DC01`, installed the Active Directory Domain Services role, and promoted it to a domain controller of a new forest, `lab.local`. DNS was installed during promotion.

![Computer name DC01](screenshots/06-computer-name.png)
![Add AD DS role](screenshots/07-add-ad-ds-role.png)
![AD DS installed](screenshots/08-ad-ds-installed.png)
![New forest deployment configuration](screenshots/09-deployment-configuration.png)
![Domain controller in Server Manager](screenshots/10-domain-controller.png)

## 3. Organizational structure and security groups

Created three organizational units (IT, HR, Sales), four test users, and a security group for each department with the matching members.

![Active Directory Users and Computers](screenshots/11-ad-users-and-computers.png)
![OU structure](screenshots/12-ou-structure.png)
![Group members](screenshots/13-group-members.png)

## 4. Account management

Practiced common help desk tasks: resetting a user's password and disabling an account.

![Reset password](screenshots/14-reset-password.png)
![Disabled account](screenshots/15-disabled-account.png)

## 5. Domain member server

Built a second server, `SRV01`, on the same internal network with a static IP address. Verified connectivity with `ping` and DNS with `nslookup lab.local`, joined it to the domain, and signed in with a domain user. The computer account appears in Active Directory.

![SRV01 desktop](screenshots/16-srv01-desktop.png)
![Network test: ping and nslookup](screenshots/17-network-test.png)
![SRV01 joined to the domain](screenshots/18-srv01-joined-domain.png)
![Domain user sign-in](screenshots/19-domain-login.png)
![SRV01 computer account in Active Directory](screenshots/20-computer-in-ad.png)

## 6. Account lockout policy

Set an account lockout threshold of 3 failed attempts in the Default Domain Policy. Triggered a lockout with wrong passwords from SRV01 and unlocked the account from the domain controller.

![Account lockout](screenshots/21-account-lockout.png)
![Unlock account](screenshots/22-unlock-account.png)

## 7. Group Policy

Created a Group Policy Object linked to the Sales OU that prohibits access to Control Panel. Tested it with a Sales user on SRV01 (access blocked) and with a user from another department (not affected).

![Group Policy Object](screenshots/23-group-policy.png)
![Policy restriction applied](screenshots/24-gpo-restriction.png)

## Skills demonstrated

- Windows Server 2022 installation and configuration
- Active Directory Domain Services and DNS
- Organizational units, users, and security groups
- Password reset, account disable, and account unlock
- Account lockout policy and Group Policy
- Static IP addressing and basic network troubleshooting (`ping`, `nslookup`, `ipconfig`)
- Joining a computer to a domain
- Virtualization with VirtualBox on limited hardware

## Troubleshooting notes

- **Windows 10 evaluation ISO unavailable:** Microsoft no longer offers it, so I used a second Windows Server 2022 as the domain member.
- **Duplicate IP address:** `ping` failed with "General failure" because SRV01 had the same IP as DC01. Assigning a unique address fixed it.
- **Virtual machine aborted state:** Using "Save machine state" left a VM in an aborted state. A normal Windows shutdown avoids this.
- **Password policy error:** A new user was rejected because the password did not meet the domain complexity policy.

## Next steps

See the [main README](../README.md) for the next labs.
