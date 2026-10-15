# Wazuh — Security Monitoring and Detection

## Overview
**Wazuh** collects and analyzes security-related events from configured endpoints. In this project, it supports log analysis, File Integrity Monitoring (FIM), and custom detection for selected LDAP authentication events.

## Lab Components
The Wazuh deployment includes the following components, where installed and enabled:
- Wazuh Manager
- Wazuh Indexer
- Wazuh Dashboard
- Postfix/SMTP notification path, where configured

Wazuh agents are deployed on selected Linux and Windows endpoints. Confirm the actual agent status in the dashboard before publishing screenshots.

## Main Functions
- Collect endpoint events and logs from configured agents.
- Analyze events using applicable decoders and rules.
- Monitor selected paths for file integrity changes.
- Apply custom rules for selected LDAP authentication events.
- Generate alerts for events matching configured rules.
- Send email notifications when the notification integration is configured and tested.

## File Integrity Monitoring (FIM)
The lab configuration included selected paths such as:
- `/etc/ssh`
- `/etc/sssd`
- `/etc/openldap`
- `/etc/sudoers.d`
- `/home`

List only paths that remain in the final configuration. Validate a file change by checking for the corresponding event in Wazuh.

## Custom LDAP Authentication Detection
Custom Wazuh decoders and rules were developed for selected LDAP authentication events. In the tested scenario, an LDAP authentication failure event could be decoded and matched against a custom rule.

The exact event format, rule ID, severity and alert outcome depend on the deployed configuration. Do not claim that every failed authentication or every brute-force attempt is detected unless tested.

## Event Flow
1. A configured endpoint generates an event.
2. The Wazuh agent forwards collected data to the Manager.
3. The Manager analyzes the event using applicable decoders and rules.
4. Matching events appear in the Dashboard.
5. If notification settings are configured, an alert can trigger an email.

## Validation
Use the Wazuh Dashboard to check:
- Agent connection and status.
- Events received from a selected endpoint.
- FIM alerts after a controlled change to a monitored file.
- A custom alert for a tested LDAP authentication failure.
- Email delivery for an alert covered by configured notification rules.

Perform tests only in the lab and avoid excessive failed logins.

## Evidence to Add
- Connected-agent overview.
- A real security event or alert.
- A FIM event for a monitored file.
- Custom LDAP detection with matching rule details.
- Received security notification with sensitive details redacted.

Suggested screenshots:
- `docs/screenshots/04-wazuh-agents.jpg`
- `docs/screenshots/05-wazuh-ldap-detection.jpg`
- `docs/screenshots/06-wazuh-fim-event.jpg`
- `docs/screenshots/07-email-notification.jpg`

## Scope and Limitations
Wazuh detects events according to collected logs, decoders, rules and configuration. Do not describe it as automatically blocking attacks unless a response mechanism was separately configured and verified. This is a lab implementation, not a claim of a complete production SOC.

## Related Components
- [LDAP](../ldap/README.md)
- [Zabbix](../zabbix/README.md)
- [Validation scenarios](../tests/validation-scenarios.md)
