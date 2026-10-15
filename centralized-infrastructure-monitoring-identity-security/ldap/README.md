# LDAP — Centralized Identity Management

## Overview
This component provides a centralized directory using **OpenLDAP**. Linux client identity lookup and authentication are integrated through **SSSD**, **PAM**, and **NSS**.

## Lab Components
- **LDAP Server:** Rocky Linux
- **Base DN:** `dc=cyberlab,dc=local`
- **Organization:** `CyberLab`
- **Client integration:** SSSD, PAM and NSS
- **Home directory creation:** `pam_mkhomedir`, where configured

> Before publishing, remove private keys, bind passwords, SMTP credentials and other sensitive information.

## Main Functions
- Store and organize directory entries centrally.
- Resolve directory users on configured Linux clients.
- Support authentication through the client's configured PAM/SSSD integration.
- Retrieve identity and group information through NSS.
- Create home directories at first login if `pam_mkhomedir` is enabled and configured.

## Authentication Flow
1. A user attempts to log in to a configured Linux client.
2. PAM handles the authentication request.
3. SSSD communicates with the LDAP directory according to its configuration.
4. NSS/SSSD provides user and group information to the system.
5. If configured, `pam_mkhomedir` creates the home directory at first successful login.

The exact flow depends on the deployed PAM, NSS, SSSD and LDAP configuration.

## Validation Examples
Run on a configured Linux client:

```bash
getent passwd rocky
id rocky
su rocky
pwd
```

These commands check whether the system can resolve the directory user and retrieve identity/group information. Test interactive login separately using a valid lab account.

## Configuration Areas
- OpenLDAP directory suffix and entries.
- SSSD domain, LDAP URI and search base.
- NSS integration.
- PAM authentication and session configuration.
- Home-directory creation, if used.
- TLS/LDAPS settings, if enabled in the final lab.

Document only configuration values verified against the final environment. Do not publish secrets.

## Evidence to Add
- Successful `getent passwd` and `id` output.
- Successful login on a configured Linux client.
- Relevant status/log output with sensitive details redacted.

Suggested screenshot: `docs\screenshots\01-ldap-user-resolution.jpg`

## Scope and Limitations
LDAP authentication applies only to systems explicitly configured to use the directory. This component does not automatically provide single sign-on for Zabbix or Wazuh web interfaces.

## Related Components
- [Zabbix](../zabbix/README.md)
- [Wazuh](../wazuh/README.md)
- [Validation scenarios](../tests/validation-scenarios.md)
