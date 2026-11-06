# Phase 08 — Azure Monitoring and Diagnostics

**Project:** Smakonnect Tech Innovation | Azure Infrastructure Administration  
**Platform:** Microsoft Azure  
**Region:** Canada Central  
**Resource Group:** `rg-smakonnect-lab-01`  
**Status:** Completed — Labs 1–7

## 1. Overview

This phase focused on implementing and validating monitoring, diagnostics, and alerting for a simulated organization's Azure infrastructure.

The lab covered infrastructure performance monitoring, centralized log collection, Kusto Query Language (KQL), Windows Event Logs, Linux Syslog, administrative activity auditing, and automated email notifications.

The objective was to develop practical Azure administration skills by configuring monitoring services, generating controlled test events, validating collected data, and investigating operational issues.

## 2. Azure Resources Used

| Resource | Name | Purpose |
|---|---|---|
| Resource Group | `rg-smakonnect-lab-01` | Hosts the lab infrastructure |
| Windows Server VM | `vm-smakonnect-win-01` | Windows performance and event monitoring |
| Ubuntu Server VM | `vm-smakonnect-linux-01` | Linux monitoring and alert testing |
| Log Analytics Workspace | `law-smakonnect-monitor-01` | Centralized log analysis |
| Windows Data Collection Rule | `dcr-smakonnect-windows-events-01` | Windows Event Log collection |
| Linux Data Collection Rule | `dcr-smakonnect-linux-syslog-01` | Linux Syslog collection |
| Action Group | `ag-smakonnect-monitor-email-01` | Email alert notifications |
| CPU Alert Rule | `alert-smakonnect-linux-high-cpu-01` | Ubuntu CPU utilization monitoring |
| Activity Log Alert | `alert-smakonnect-linux-deallocate-01` | VM deallocation notifications |
| Diagnostic Setting | `diag-smakonnect-activity-logs-01` | Subscription Activity Log export |

## 3. Lab 1 — Azure Monitor Metrics

Explored Azure Monitor Metrics to understand how Azure provides performance telemetry for virtual machines.

Reviewed CPU utilization, network activity, and disk performance metrics using the Azure Portal.

**Skills demonstrated:**
- Selecting monitored resources and metric namespaces
- Interpreting performance graphs
- Understanding metric aggregation and time ranges
- Using performance data for infrastructure troubleshooting

## 4. Lab 2 — Log Analytics and KQL

Configured and explored the Log Analytics workspace `law-smakonnect-monitor-01`.

The workspace was configured with:
- Analytics log retention: 30 days
- Daily ingestion cap: 0.023 GB/day
- Centralized collection of supported monitoring data

Used KQL to query and analyze collected records.

This exercise demonstrated the difference between platform metrics and log-based monitoring, including the need to configure data collection before certain log tables become populated.

## 5. Lab 3 — Windows Event Log Monitoring

Configured Windows Event Log collection through Azure Monitor Agent and a Data Collection Rule.

**Data Collection Rule:** `dcr-smakonnect-windows-events-01`

Configured collection for selected System and Application events, with filtering designed to reduce unnecessary ingestion.

### Validation

Restarted the Windows Print Spooler service to generate a test event.

Verified that Event ID **7036** was successfully collected in the Log Analytics `Event` table.

```kusto
Event
| where EventID == 7036
| project TimeGenerated, Computer, EventLog,
          EventID, Source, RenderedDescription
| order by TimeGenerated desc
```

**Result:** Windows Event Log ingestion successfully validated.

After testing, the DCR was restored to its intended Critical/Error filtering configuration.

## 6. Lab 4 — Linux Syslog Monitoring

Configured Linux Syslog collection using Azure Monitor Agent and the Data Collection Rule:

`dcr-smakonnect-linux-syslog-01`

Configured selected Syslog facilities:
- auth
- authpriv
- daemon
- syslog

Collection was limited to Warning severity and above.

### Validation

Generated a controlled Syslog event on the Ubuntu VM:

```bash
logger -p daemon.warning -t SmakonnectLab \
"AZURE-MONITOR-TEST: Linux Syslog monitoring verification"
```

Queried the Log Analytics `Syslog` table:

```kusto
Syslog
| where SyslogMessage contains "AZURE-MONITOR-TEST"
| project TimeGenerated, Computer, Facility,
          SeverityLevel, SyslogMessage
| order by TimeGenerated desc
```

**Result:** The generated Linux Syslog event was successfully collected and retrieved.

## 7. Lab 5 — Azure Monitor CPU Alerting

Created a metric alert rule for the Ubuntu VM.

**Alert rule:** `alert-smakonnect-linux-high-cpu-01`

**Configuration:**
- Metric: Percentage CPU
- Threshold: Greater than 20%
- Aggregation: Average
- Aggregation period: 5 minutes
- Evaluation frequency: 5 minutes
- Severity: 2
- Action Group: `ag-smakonnect-monitor-email-01`

### Validation

Generated a controlled CPU workload using:

```bash
stress-ng --cpu 1 --cpu-load 70 \
--timeout 8m --metrics-brief
```

Observed CPU utilization exceeding the configured threshold.

Azure Monitor evaluated the metric condition, triggered the alert, and delivered an email notification through the configured Action Group.

**Result:** Successfully validated the monitoring-to-notification workflow.

## 8. Lab 6 — Azure Activity Logs and Administrative Monitoring

Investigated Azure Activity Logs to understand subscription-level administrative operations.

Reviewed VM start and deallocation events, alert configuration changes, and failed resource operations.

### VM Allocation Failure Investigation

Identified a failed Ubuntu VM start operation with the error:

`AllocationFailed`

The error indicated insufficient Azure capacity for the requested VM size in the selected region.

This demonstrated how Activity Logs can support troubleshooting of Azure control-plane operations.

The VM was subsequently observed running, but the exact remediation for the earlier allocation failure was not independently established.

### Exporting Administrative Logs

Created the subscription diagnostic setting:

`diag-smakonnect-activity-logs-01`

Configured the **Administrative** log category to be exported to:

`law-smakonnect-monitor-01`

Validated that administrative operations appeared in the `AzureActivity` table.

```kusto
AzureActivity
| where TimeGenerated > ago(7d)
| where CategoryValue == "Administrative"
| project TimeGenerated, OperationNameValue,
          ActivityStatusValue, Caller,
          ResourceGroup
| order by TimeGenerated desc
```

### VM Deallocation Alert

Created:

`alert-smakonnect-linux-deallocate-01`

Configured the rule to monitor successful deallocation of `vm-smakonnect-linux-01`, using the existing email Action Group.

### Validation

Stopped and deallocated the Ubuntu VM.

Confirmed:
- VM entered the Stopped (deallocated) state
- Azure Monitor delivered an alert email
- The deallocation operation appeared in Log Analytics

```kusto
AzureActivity
| where TimeGenerated > ago(7d)
| where OperationNameValue =~
    "Microsoft.Compute/virtualMachines/deallocate/action"
| where ActivityStatusValue =~ "Success"
| project TimeGenerated, OperationNameValue,
          ActivityStatusValue, Caller,
          ResourceId, CorrelationId
| order by TimeGenerated desc
```

**Result:** Successfully validated administrative activity monitoring, log export, and event-driven email notification.

## 9. Lab 7 — Azure Resource Health and Service Health

Reviewed Azure Resource Health for the Windows and Ubuntu virtual machines.

Explored Azure Service Health to understand how Azure communicates platform incidents and service availability information.

### Validation

Checked the Service Health dashboard.

**Observed result:** No active service issues were displayed at the time of inspection.

This was recorded as a point-in-time observation, not evidence that Azure had never experienced incidents.

Resource Health and Service Health were compared to understand the distinction between individual resource availability and broader Azure platform incidents.

No additional Service Health alert was configured during this exercise.

## 10. Troubleshooting and Lessons Learned

| Scenario | Investigation or resolution |
|---|---|
| Windows Event Log collection | Generated and verified a known service-control event |
| Linux Syslog collection | Generated a Warning-level event and confirmed ingestion |
| CPU alert validation | Generated controlled CPU load and confirmed email delivery |
| Azure VM allocation failure | Inspected Activity Log JSON and identified a capacity-related error |
| Empty `AzureActivity` query | Configured subscription diagnostic export to Log Analytics |
| Activity Log query filtering | Verified that ingested successful operations use `Success` in `ActivityStatusValue` |
| VM deallocation alert | Confirmed the VM state, email notification, and corresponding log record |
| Service Health investigation | Reviewed the dashboard and observed no active service issues |

## 11. Security and Cost Considerations

Monitoring was configured with attention to the lab's limited Azure subscription credits.

Key considerations included:
- Filtering Windows Event Logs and Linux Syslog to reduce ingestion
- Setting a Log Analytics daily ingestion cap
- Configuring a 30-day log retention period
- Reusing an existing Action Group
- Deallocating virtual machines when not required
- Reviewing temporary alert rules after validation
- Avoiding unnecessary monitoring resources

Temporary alert rules and diagnostic settings should be reviewed periodically for ongoing charges.

## 12. Skills Demonstrated

- Azure Monitor Metrics
- Log Analytics workspace administration
- Kusto Query Language (KQL)
- Azure Monitor Agent
- Data Collection Rules
- Windows Event Log monitoring
- Linux Syslog monitoring
- Metric and Activity Log alerts
- Action Groups and email notifications
- Azure Activity Log investigation
- Diagnostic settings
- Azure Resource Health and Service Health
- Infrastructure troubleshooting
- Monitoring cost optimization

## 13. Conclusion

Phase 8 established practical monitoring and diagnostics capabilities for the Smakonnect Tech Innovation Azure environment.

The exercises demonstrated how to collect infrastructure telemetry, analyze Windows and Linux logs, detect resource conditions, investigate administrative operations, and validate alert notifications through controlled testing.

These activities reflect foundational responsibilities of Azure Administrators, Cloud Support Engineers, and Infrastructure Support professionals working with Microsoft Azure.

**Project type:** Hands-on learning and portfolio lab — not a production deployment.
