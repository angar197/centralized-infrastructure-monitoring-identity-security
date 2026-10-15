# Zabbix — Infrastructure Monitoring

## Overview
**Zabbix** monitors infrastructure availability, system resources and selected services. The lab uses a Zabbix server with a MariaDB database and a web interface.

## Lab Components
- **Zabbix Server OS:** Ubuntu Server 24.04.4
- **Monitoring platform:** Zabbix 7.0
- **Database:** MariaDB
- **Endpoints:** configured Linux and Windows systems
- **Additional monitoring:** SNMP for a configured target, where enabled
- **Notifications:** email notifications for selected triggers, where configured

## Main Functions
Depending on the host and checks configured, Zabbix can monitor:
- CPU utilization and system load.
- Memory and swap usage.
- Disk space and disk I/O.
- Network traffic.
- Host and agent availability.
- Selected service states.
- SNMP-provided information for supported targets.

Exact coverage depends on the templates, items, agents and SNMP settings applied to each host.

## Architecture and Data Flow
1. Agents run on configured endpoints.
2. The Zabbix server collects monitoring data.
3. MariaDB stores Zabbix configuration and monitoring history.
4. The web interface presents hosts, latest data, graphs, problems and events.
5. Configured triggers can generate events and, when actions and media settings are correct, email notifications.

## Validation
On an endpoint, check the agent service name appropriate to the installed package. For example:

```bash
sudo systemctl status zabbix-agent
```

Then use the Zabbix web interface to verify:
- Host availability.
- Recent values in Latest data.
- Graphs for selected metrics.
- Problem and recovery events for configured triggers.
- Email delivery for a configured test notification.

For SNMP, use the correct target, SNMP version, credentials/community and OID. Do not publish SNMP secrets.

## Service Availability Test
On a lab system where interruption is safe, stop a service explicitly monitored by an item or trigger, observe the event, restart the service and confirm recovery if configured.

Stopping a service will not necessarily create an alert unless the relevant item, trigger and evaluation conditions are configured.

## Email Notifications
Delivery requires a configured media type, recipient settings, action conditions and a working mail relay/SMTP configuration. Document only notifications that were actually tested.

## Evidence to Add
- Zabbix dashboard or host overview.
- Latest data/graphs for CPU, memory, disk and network.
- Service problem and recovery event.
- Received notification email with personal information redacted.
- SNMP output or corresponding Zabbix item, if used.

Suggested screenshots:
- `docs/screenshots/02-zabbix-dashboard.jpg`
- `docs/screenshots/03-zabbix-service-alert.jpg`
- `docs/screenshots/07-email-notification.jpg`

## Scope and Limitations
Zabbix provides monitoring and alerting. It does not automatically remediate problems unless explicit actions or external automation have been configured.

## Related Components
- [LDAP](../ldap/README.md)
- [Wazuh](../wazuh/README.md)
- [Validation scenarios](../tests/validation-scenarios.md)
