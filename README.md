Dcdiag
Introduction

Dcdiag (Domain Controller Diagnostics) is a built-in Microsoft Windows Server command-line utility designed to diagnose, test, and troubleshoot Active Directory Domain Services (AD DS) environments. It helps system administrators identify domain controller failures, verify Active Directory health, and investigate problems affecting domain authentication, replication, DNS configuration, and network connectivity. The tool is particularly useful for maintaining Windows Server domain infrastructure and ensuring reliable communication between domain controllers.

The utility performs a series of diagnostic tests against local or remote domain controllers and evaluates critical components of an Active Directory deployment. Its tests cover essential services and dependencies, including directory replication, DNS registration, advertising, security authentication, Netlogon, SYSVOL, and domain controller availability. By analyzing test results, administrators can detect configuration errors, service interruptions, replication inconsistencies, and other conditions that may affect domain operations.

Dcdiag supports individual diagnostic tests, comprehensive health checks, targeted server examinations, and verbose output for investigating specific failures. Administrators can specify a domain controller, test category, or domain context to narrow the scope of troubleshooting. This flexibility makes it suitable for routine Active Directory health assessments, post-deployment verification, infrastructure migrations, and incident investigation.

The utility is available on supported Windows Server installations with Active Directory management tools and can also be used from systems with the appropriate Remote Server Administration Tools (RSAT) components installed. It is executed through Command Prompt or PowerShell with sufficient permissions for the requested diagnostic operations.

For IT professionals, this tool provides a practical way to validate domain controller functionality, investigate Active Directory errors, and distinguish between directory service problems and underlying DNS, network, or replication issues.

How to Run Dcdiag Commands and Perform Active Directory Health Checks

Dcdiag is executed from an elevated Command Prompt or PowerShell session. Running the command without additional parameters performs a standard set of diagnostic tests against the local domain controller. To examine a specific server, use the /s parameter followed by its hostname:

cmd
dcdiag /s:DC01


To perform a comprehensive diagnostic assessment of domain controllers in an Active Directory domain, use:

cmd
dcdiag /e


The /e parameter extends testing to enterprise domain controllers, while /a targets all domain controllers in the current site. These options are useful when investigating infrastructure-wide issues rather than failures isolated to one server.

Administrators can select individual tests to investigate particular Active Directory components. For example:

cmd
dcdiag /test:DNS /s:DC01
dcdiag /test:Advertising /s:DC01
dcdiag /test:Services /s:DC01
dcdiag /test:Replications /s:DC01


The DNS test checks important DNS-related conditions, including name resolution and service records. Advertising verifies whether a domain controller correctly advertises its roles and services. Services checks the operational state of required Windows services, while Replications examines Active Directory replication status.

For more detailed diagnostic information, use:

cmd
dcdiag /v


To save diagnostic results for further analysis, redirect the output:

cmd
dcdiag /e /v > C:\Temp\dcdiag-results.txt


This approach is useful for scheduled health checks, maintenance reports, and troubleshooting records. When interpreting results, administrators should investigate failed tests in context, because a diagnostic failure may indicate an underlying DNS, connectivity, permissions, or replication problem rather than a defect in the diagnostic utility itself.

How to Troubleshoot Active Directory DNS, Replication, and Domain Controller Errors

One of the most important applications of Dcdiag is troubleshooting Active Directory domain controller failures. DNS configuration, directory replication, and domain controller discovery are closely connected. Problems in any of these areas can cause authentication failures, Group Policy processing errors, and difficulties accessing domain resources.

DNS diagnostics are particularly valuable when clients cannot locate domain controllers or when domain controllers fail to register their service records. Run:

cmd
dcdiag /test:DNS /v


Review the output for failed DNS registration, missing SRV records, name resolution problems, and connectivity issues. Verify that domain controllers use the appropriate internal DNS servers and that Active Directory-related DNS zones contain the required records.

Replication problems can be investigated with:

cmd
dcdiag /test:Replications /v


A failed replication test may indicate network interruptions, DNS resolution errors, authentication problems, or unavailable replication partners. Administrators should correlate the results with the Windows Directory Service event log and use repadmin /replsummary and repadmin /showrepl for more detailed replication analysis.

To investigate whether a domain controller is correctly operating and advertising its services, run:

cmd
dcdiag /test:Advertising /v
dcdiag /test:SysVolCheck /v
dcdiag /test:NetLogons /v


These tests help identify problems involving domain controller advertising, SYSVOL readiness, and Netlogon resource availability. Such failures may affect Group Policy distribution, logon scripts, and domain authentication.

For systematic troubleshooting, first identify the failing diagnostic test, then examine the relevant Windows services, DNS configuration, event logs, and network connectivity. After correcting the underlying issue, rerun the affected tests to verify recovery. This method helps administrators isolate Active Directory infrastructure problems and confirm that domain controllers are functioning correctly.
