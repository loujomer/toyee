
requires a Microsoft Entra ID Premium P2 license

|**Risk detection type**|**Description**|
|---|---|
|Anonymous IP address|Sign in from an anonymous IP address (for example: Tor browser, anonymizer VPNs).|
|Atypical travel|Sign in from an atypical location based on the user's recent sign ins.|
|Malware-linked IP address|Sign in from a malware-linked IP address.|
|Unfamiliar sign in properties|Sign in with properties we've not seen recently for the given user.|
|Leaked credentials|Indicates that the user's valid credentials have been leaked.|
|Password spray|Indicates that multiple usernames are being attacked using common passwords in a unified brute-force manner.|
|Microsoft Entra threat intelligence|Microsoft's internal and external threat intelligence sources have identified a known attack pattern.|
|New country|This detection is discovered by Microsoft Defender for Cloud Apps (MDCA).|
|Activity from anonymous IP address|This detection is discovered by MDCA.|
|Suspicious inbox forwarding|This detection is discovered by MDCA.|

## License requirements

|**Capability**|**Details**|**Microsoft Entra ID Free / Microsoft 365 Apps**|**Microsoft Entra ID Premium P1**|**Microsoft Entra ID Premium P2**|
|---|---|---|---|---|
|Risk policies|User risk policy (via Identity Protection)|No|No|Yes|
|Risk policies|Sign-in risk policy (via Identity Protection or Conditional Access)|No|No|Yes|
|Security reports|Overview|No|No|Yes|
|Security reports|Risky users|Limited information. Only users with medium and high risk are shown. No details drawer or risk history.|Limited information. Only users with medium and high risk are shown. No details drawer or risk history.|Full access|
|Security reports|Risky sign ins|Limited information. No risk detail or risk level is shown.|Limited information. No risk detail or risk level is shown.|Full access|
|Security reports|Risk detections|No|Limited information. No details drawer.|Full access|
|Notifications|Users at risk detected alerts|No|No|Yes|
|Notifications|Weekly digest|No|No|Yes|
|MFA registration policy | |No|No|Yes|


There are two risk policies that can be enabled in the directory:

- **Sign-in risk policy**
	- remediation: Require MFA
- **User risk policy**
	- remediation: Require password change

---
# Monitor, investigate, and remediate elevated risky users

## Investigate risk

report types of identity risk:
- risky users
- risky sign-ins 
- risk detections
- risky workload identities

## Remediate risks and unblock users

- All active risk detections contribute to the calculation of a value called _user risk level_.

##### Remediate options:
- Self-remediation with risk policy
- Manual password reset
- Dismiss user risk
- Close individual risk detections manually

