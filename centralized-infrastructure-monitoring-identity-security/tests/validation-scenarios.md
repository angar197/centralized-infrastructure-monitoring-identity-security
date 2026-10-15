# Validation Scenarios

## Purpose
This document describes functional scenarios for validating LDAP identity management, Zabbix monitoring, Wazuh security monitoring and configured email notifications.


## Scenario 1 — Centralized LDAP User Lookup and Authentication

**Objective:** Verify that a configured Linux client resolves a directory user and authenticates through the configured LDAP/SSSD/PAM integration.

**Prerequisites:** The server is reachable, the client is configured for the directory, and a valid test account exists.

**Steps:**
1. Run `getent passwd rocky` on the client.
2. Run `id rocky`.
3. Test interactive login with the lab account.
4. If first-login home creation is in scope, verify that behavior.

**Expected result:** User/group information is returned; authentication succeeds with valid credentials; a home directory is created if `pam_mkhomedir` is configured and applicable.

**Evidence:** `docs\screenshots\01-ldap-user-resolution.jpg`

**Actual result:** PASS for user resolution, provided the expected user and group information appears

**status:**  PASS

## Scenario 2 — Zabbix Host and Resource Monitoring

**Objective:** Verify that Zabbix receives data from a configured endpoint.

**Steps:**
1. Open the Zabbix web interface.
2. Check host availability.
3. Open Latest data or relevant graphs.
4. Verify recent values for selected metrics such as CPU, memory, disk or network.

**Expected result:** The host is available according to configured checks and selected items return recent data.

**Evidence:** `docs/screenshots/02-zabbix-dashboard.jpg`

**Actual result:** the test shows the monitored host and valid, recent metrics .

**status:** PASS

## Scenario 3 — Detection of a Monitored Service Being Unavailable

**Objective:** Verify a configured Zabbix trigger for a service state.

**Prerequisites:** A service item/check and trigger exist; the test is performed on a lab system where interruption is safe.

**Steps:**
1. Record the initial service state.
2. Stop the test service.
3. Wait for the configured check and trigger evaluation.
4. Confirm the problem event in Zabbix.
5. Restart the service.
6. Confirm recovery if recovery conditions are configured.

**Expected result:** Zabbix raises a problem when the configured condition is met ,send an email notification and returns to normal after recovery if configured.

**Evidence:** `docs/screenshots/03-zabbix-service-alert.jpg`

**Actual result:** The notification email was successfully received after the service was stoped.

**status:** PASS

## Scenario 4 — File Integrity Monitoring (FIM)

**Objective:** Verify that Wazuh detects a change to a file or directory configured for monitoring.

**Steps:**
1. Select a harmless test file in a monitored path, where permitted.
2. Record its initial state.
3. Make a controlled change.
4. Wait for the configured FIM scan or real-time event.
5. Search for the event in the Wazuh Dashboard.

**Expected result:** Wazuh records the change if the path and event type are covered by the configuration.

**Evidence:** `docs/screenshots/06-wazuh-fim-event.jpg`

**Actual result:** Wazuh detect the changes  to a file  configured for monitoring.

**status:** PASS

## Scenario 5 — Detection of a Selected LDAP Authentication Failure

**Objective:** Verify the custom Wazuh decoder/rule for a supported LDAP authentication failure event.

**Prerequisites:** The event is logged, collected by an agent, and the custom decoder/rule is installed and loaded.

**Steps:**
1. Perform a controlled failed authentication attempt against the lab LDAP service.
2. Verify the event in the source log.
3. Verify Wazuh receives it.
4. Inspect the alert and identify the matching decoder/rule.

**Expected result:** The event is collected and matches the custom rule if its format and conditions satisfy that rule.

**Evidence:** `docs/screenshots/05-wazuh-ldap-detection.jpg`

**Actual result:** Wazuh detect the LDAP authentication failure event.

**status:** PASS

## Scenario 6 — Email Notification

**Objective:** Verify that a configured alert is delivered to the administrator's mailbox.

**Prerequisites:** SMTP/relay settings, alerting action, recipient and media settings are configured; the test event matches the action conditions.

**Steps:**
1. Trigger a controlled test event.
2. Confirm the alert in Zabbix or Wazuh.
3. Check the notification configuration.
4. Verify delivery in the receiving mailbox.
5. Redact personal information before capturing evidence.

**Expected result:** An email is received if all alerting and mail-delivery conditions are satisfied.

**Evidence:** `docs/screenshots/07-email-notification.jpg`

**Actual result:** The notification email was successfully received after the alert was triggered.

**status:** PASS

## Scenario 7 — SNMP Monitoring 

**Objective:** Verify that a configured SNMP target responds and/or that Zabbix receives the configured SNMP item.

**Steps:**
1. Confirm the target, SNMP version and credentials used in the lab.
2. Run a query with the configured parameters without exposing secrets.
3. Confirm that the expected OID responds.
4. If integrated into Zabbix, verify the corresponding item data.

**Expected result:** The target returns the requested information when SNMP is enabled and connectivity and credentials are correct.

**Evidence:** `docs/screenshots/08-SNMP-Connectivity-test.jpg`

**Actual result:** the test shows configured SNMP target responds and that Zabbix receives the configured SNMP item.

**status:** PASS

## Integration Summary

| Area | What to verify | Evidence |
|---|---|---|
| LDAP | User lookup and configured authentication | Client output / login test |
| Zabbix | Host availability and recent metrics | Dashboard / Latest data |
| Service monitoring | Problem and recovery behavior | Problem event history |
| Wazuh | Agent connectivity and event ingestion | Agent overview / event view |
| FIM | Detection of a monitored file change | FIM alert |
| Custom detection | Selected LDAP event matches a rule | Alert details |
| Email | Test alert is delivered | Redacted email screenshot |
| SNMP | Query/item works, if configured | Terminal output / item data |

## Notes
- Mark a scenario `PASS` only when observed results match the expected result.
- A diagram explains intended behavior; it is not a substitute for execution evidence.
- Never commit passwords, SMTP credentials, API keys, private keys, sensitive logs or personal data.
