# DAT151: Assignment 5 Report

**Group:** 5

**Group Members:** Soukup Jan, Fabienne Feilke

**Date:** March 2026

---

## Task 1: Logging

### a) Configure systemd journal
*Configure systemd journal at the lab to save the logs persistent in the file system.*

**Solution:**
<!-- Add commands and explanation here (e.g., enable Storage=persistent in /etc/systemd/journald.conf) -->


### b) Protect journal logs
*Protect the journal logs from unnoticed alteration by enabling the Seal feature and create a sealing key. Verify the logs and prove that the logs are sealed. The verification output will show a time interval that is not sealed.*

**Solution:**
<!-- Add commands to enable Seal=yes, create key, and verify logs (e.g., journalctl --setup-keys, journalctl --verify) -->


### c) Syslog local5 facility
*The syslog facility names “local0” to “local7” are used for local custom messages defined by an administrator. Create your own configuration that saves only the critical level and information level messages from the local5 facility to a file local5 in the /var/log directory. Test your configuration using the logger command with specific messages. Look under the /var/log directory and verify that the messages you added are saved at the correct location.*

**Solution:**
<!-- Add rsyslog.conf changes, logger commands, and output from /var/log/local5 -->


---

## Task 2: LDAP

*In this task you will set up an LDAP server and client(s). Use one of the computers of the group as the LDAP server, and the others as clients.*

### Installation and Configuration
*Document your configurations clearly, include your LDIF files in your report, and explain how user records were added to your server.*

**Solution:**
<!-- Explain installation steps (packages: openldap-clients, openldap-servers; service: slapd) -->


### DN Suffix and Manager Password
*Configure the database with the new DNs (base DN and administrator DN) and set up the manager password and access.*

**Base DN LDIF (`basedn.ldif`):**
```ldif
<!-- Paste content of basedn.ldif here -->
```

**Manager Config LDIF (`manager.ldif`):**
```ldif
<!-- Paste content of manager.ldif here -->
```

**Explanation:**
<!-- Explain the changes made (RootDN, Suffix, Password hash creation) -->


### Schemas
*Install necessary schemas (COSINE, NIS).*

**Solution:**
<!-- Commands used to install schemas: cosine.ldif, nis.ldif -->


### Create and Fill User Records Database
*Create the database for user records and fill it with users and groups.*

**Database DN LDIF (`top.ldif`):**
```ldif
<!-- Paste content of top.ldif here -->
```

**User/Group Creation:**
<!-- Explain how users/groups were created. Include LDIFs or commands used (ldapadd, etc). -->

**Verification:**
<!-- Output of ldapsearch demonstrating the user/group exists -->
<!-- Example: ldapsearch -LLL -x -b "dc=...,dc=..." -->


---

## Task 3: SSSD

*Configure SSSD to use the LDAP from Task 2 for user authentication. You will also configure LDAPS (LDAP over SSL/TLS).*

### SSSD Configuration
*Configure `/etc/sssd/sssd.conf`, use `authselect` to select the PAM profile for SSSD, and enable `with-mkhomedir`.*

**Solution:**
<!-- Content of /etc/sssd/sssd.conf and authselect commands usage -->


### Login Verification (Client)
*The client computer should be a different computer than the LDAP host, and the user should not exist on the client prior to log in. You must must a add screen shot to the report of a working log in on the client: `su - <some_user>` (To be run on the client, NOT the LDAP host).*

**Screenshot:**
<!-- Paste screenshot or terminal output of successful login -->
![Login Verification](path/to/image.png)


### LDAP Access Control
*With login working from the client using SSSD, the LDAP access rules should be restricted further by modifying the LDAP `olcAccess` parameter. See e.g. the man pages slapd-config(5) and slapd.access(5).*

**Solution:**
<!-- LDIF or commands used to modify olcAccess to restrict access -->


### LDAPS Configuration
*Configure the LDAP server to use the encrypted `ldaps` channel. Create/Sign certificates, configure standard hostname resolution (/etc/hosts), and update LDAP configuration.*

**Certificate Setup:**
<!-- Explain how certificates were created/signed (openssl, CA setup) -->

**LDAP Cert Configuration:**
<!-- LDIF used to add olcTLSCertificateFile and olcTLSCertificateKeyFile to cn=config -->

**SSSD Update:**
<!-- Changes to sssd.conf to use ldaps URI and CA certificate -->


---

## Task 4: Kerberos

*Configure a host as a KDC (Key Distribution Center) and also use it as a Kerberos client to authenticate SSH logins.*

### KDC Configuration
*Configure KDC, create the database, and enable services (`kadmin`, `krb5kdc`). Create the KDC database with a secure password.*

**Solution:**
<!-- Explanations and commands for KDC setup -->


### Principals
*Create necessary principals (User, Admin, Host, Service). Since only root can run kadmin.local, and the default principal of root is “root/admin”, create an administrator principal “root/admin”.*

**Principals Created:**
*   User: `...` (e.g., student@REALM)
*   Admin: `...` (e.g., root/admin@REALM)
*   Host: `...` (e.g., host/hostname@REALM)

**Commands:**
<!-- Commands used to create principals (kadmin.local addprinc ...) -->


### SSH with Kerberos
*Configure SSH to use Kerberos. Verify login in both directions.*

**Configuration Changes:**
<!-- Changes to /etc/ssh/sshd_config (GSSAPIAuthentication yes, etc.) and /etc/krb5.conf -->

**Verification:**
<!-- Output showing successful SSH login using Kerberos tickets (klist output, ssh -v output showing gssapi-with-mic) -->
