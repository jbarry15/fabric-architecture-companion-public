# Fabric Security Anti-Patterns

Common security mistakes in Microsoft Fabric deployments and how to avoid them.

---

## 1. Over-Permissive Workspace Roles

### ❌ Anti-Pattern

Assigning **Admin** or **Member** roles to all users in a workspace for convenience, giving everyone the ability to modify items, manage access, and delete content.

### Why It's Dangerous

- Users can accidentally delete or overwrite critical items
- Shared access cannot be revoked granularly
- Violates least-privilege and audit compliance requirements
- One compromised account gains full workspace control

### ✅ Correct Approach

- Default to **Viewer** role; elevate only when justified
- Use **Contributor** for builders who don't need to manage access
- Use Entra security groups (not individuals) for role assignments
- Review and right-size roles quarterly

---

## 2. No Row-Level Security on Shared Datasets

### ❌ Anti-Pattern

Publishing semantic models that serve multiple teams or business units without implementing RLS, relying on "users won't look at data that isn't theirs."

### Why It's Dangerous

- Any viewer can query all data via Analyze in Excel, XMLA, or DAX Studio
- Regulatory violations (GDPR, HIPAA) when PII is exposed cross-boundary
- No audit trail of who accessed which data subset

### ✅ Correct Approach

- Implement RLS with dynamic security (`USERPRINCIPALNAME()`) wherever data has row-level boundaries
- Test with "View As Role" before publishing
- For highly sensitive data, use separate datasets per audience
- Document RLS rules alongside the semantic model

---

## 3. Sharing Items with "Entire Organization"

### ❌ Anti-Pattern

Using the "Share with entire organization" option or broad distribution lists for reports and datasets containing sensitive data.

### Why It's Dangerous

- Any employee (or guest with tenant access) can view the data
- Sensitivity labels alone don't prevent viewing — only control downstream actions
- Difficult to audit who actually accessed the content
- Creates sprawl of uncontrolled data access

### ✅ Correct Approach

- Share with specific security groups that map to data access requirements
- Use Power BI Apps with defined audiences for broad distribution
- Enable "Build" permission only for users who need to create derivative content
- Periodically audit sharing via the Admin API or Fabric scanner

---

## 4. Exposing Data Sources Without Network Controls

### ❌ Anti-Pattern

Opening public endpoints on Azure SQL, ADLS Gen2, or other data sources so Fabric can connect, instead of using private connectivity.

### Why It's Dangerous

- Data sources are exposed to the internet
- Any entity with credentials could connect (not just Fabric)
- Violates zero-trust network principles
- May violate compliance requirements for regulated workloads

### ✅ Correct Approach

- Use **Managed Private Endpoints** for Spark workloads
- Use **Trusted Workspace Access** for ADLS Gen2
- Use **VNet Data Gateways** for Azure-resident sources
- Use **Service Tags** when private endpoints aren't feasible
- Keep data sources firewalled; allowlist only the necessary Fabric connectivity path

---

## 5. Skipping Sensitivity Labels

### ❌ Anti-Pattern

Not applying Microsoft Purview sensitivity labels to Fabric items, or applying them inconsistently ("we'll label later").

### Why It's Dangerous

- No downstream protection — DLP policies can't act on unlabeled content
- Labels propagate automatically in Fabric; missing source labels means downstream items are unprotected
- Compliance teams have no visibility into data classification
- Export controls and watermarking don't activate without labels

### ✅ Correct Approach

- Enable **mandatory labeling** in the Fabric tenant settings
- Define a default label (e.g., "General") so nothing goes unlabeled
- Train data creators on proper classification at ingestion time
- Automate label inheritance from source data systems where possible

---

## 6. Using Personal Accounts for Automation

### ❌ Anti-Pattern

Configuring scheduled refreshes, pipelines, or API integrations using a named user account instead of a service principal.

### Why It's Dangerous

- When the user leaves or changes password, all automations break
- User accounts may have MFA requirements that block non-interactive flows
- Impossible to scope access narrowly — user accounts inherit all their personal permissions
- Audit logs show a user name, making it hard to distinguish automated vs. manual actions

### ✅ Correct Approach

- Use **service principals** for all automation (pipelines, API calls, refresh schedules)
- Create dedicated Entra app registrations per workload boundary
- Store credentials in Azure Key Vault (not in pipeline parameters)
- Scope service principal workspace access to only what's needed

---

## 7. Ignoring Audit Logs

### ❌ Anti-Pattern

Not configuring or reviewing Fabric audit logs, relying solely on "we'll investigate if something goes wrong."

### Why It's Dangerous

- Breaches go undetected for weeks or months
- No forensic trail for compliance investigations
- Can't identify anomalous access patterns (bulk exports, off-hours activity)
- Regulatory fines for insufficient logging (SOX, HIPAA, GDPR)

### ✅ Correct Approach

- Route audit logs to a centralized SIEM or Log Analytics workspace
- Define alerts for critical events: admin role changes, external sharing, bulk downloads
- Review high-sensitivity access logs weekly
- Retain logs for at least 90 days (longer for regulated industries)
- Use Microsoft Defender for Cloud Apps for real-time session monitoring

---

## 8. Tenant-Wide Private Link Without Testing

### ❌ Anti-Pattern

Enabling tenant-level Private Link and blocking public access without validating all connectivity paths first, or using tenant-level controls when workspace-level Private Link would isolate only the sensitive workspaces.

### Why It's Dangerous

- Tenant-level Private Link applies to **all workspaces**; workspace-level Private Link is available when only selected workspaces need private inbound access
- Breaks access for remote workers, partners, and SaaS integrations not on the private network
- On-premises data gateways may lose connectivity
- Embedded reports and public APIs stop working
- Rollback can be disruptive

### ✅ Correct Approach

- Inventory all access paths before enabling (remote users, gateways, embedded, APIs)
- Prefer workspace-level Private Link for targeted sensitive workspaces; use tenant-level Private Link only when the whole tenant requires private inbound access
- Test in a non-production tenant/workspace or during a maintenance window
- Ensure VPN/ExpressRoute connectivity for all user populations
- Plan fallback procedures before flipping the switch
- Communicate changes to all stakeholders with lead time

---

## 9. Copying Production Data to Dev Without Anonymization

### ❌ Anti-Pattern

Refreshing development or test workspaces from production data sources, giving developers access to real PII/PHI without masking.

### Why It's Dangerous

- Developers may not have authorization to view production PII
- Dev workspaces typically have weaker security controls
- Increases blast radius if a dev environment is compromised
- Violates data minimization principles (GDPR Art. 5)

### ✅ Correct Approach

- Use synthetic or anonymized datasets for dev/test
- If production data is required, apply dynamic data masking or subsetting
- Ensure dev workspaces have equivalent sensitivity labels to production
- Use deployment pipelines with rules that strip PII when promoting backward

---

## 10. Relying on Obscurity Instead of Controls

### ❌ Anti-Pattern

Assuming that because a workspace or item isn't shared explicitly, it's secure. "Nobody knows the URL" or "it's buried in a workspace they'd have to search for."

### Why It's Dangerous

- Fabric Admin APIs and scanner can enumerate all items in the tenant
- Users with Fabric licenses can discover workspaces through search
- A single misconfigured sharing action exposes the content
- Security through obscurity is not a recognized control in any compliance framework

### ✅ Correct Approach

- Apply explicit access controls to every workspace and item
- Use sensitivity labels as a second layer of protection
- Assume all content is discoverable — secure it accordingly
- Regularly run access reviews using the Admin API or Microsoft Purview

---

## 11. Securing Only the Semantic Model, Not the Data Layer

### ❌ Anti-Pattern

Implementing RLS/CLS only in the Power BI semantic model while leaving the underlying lakehouse/warehouse data in OneLake open, assuming "users only reach the data through the report."

### Why It's Dangerous

- Semantic-model RLS protects the model **only** — it does nothing for direct access paths
- Users with lakehouse access can bypass it via Spark notebooks, the SQL analytics endpoint, OneLake APIs, or external engines
- The same table ends up with inconsistent, per-engine security rules that drift over time
- Regulatory exposure (GDPR/HIPAA) when PII is readable through an unsecured path

### ✅ Correct Approach

- Apply **OneLake security** (data-plane) so table/folder, row-, and column-level rules are defined at the data layer and enforced across supported Spark, T-SQL, Direct Lake on OneLake, and authorized external-engine paths
- Use OneLake security roles (**type / permission / scope / members**) for readers; keep Admin/Member/Contributor membership small since those roles get elevated OneLake access and override reader restrictions
- Review the lakehouse `DefaultReader` role and remove unintended broad `ReadAll` access
- Layer semantic-model RLS/OLS on top for BI convenience — but never as the *only* control for sensitive underlying lakehouse data

> **Reference**: [OneLake data security overview](https://learn.microsoft.com/en-us/fabric/onelake/security/get-started-security) · [Best practices for securing data in OneLake](https://learn.microsoft.com/en-us/fabric/onelake/security/best-practices-secure-data-in-onelake)

---

## References

- [Microsoft Fabric Security White Paper (PDF)](https://aka.ms/fabricsecuritywhitepaper)
- [Security in Microsoft Fabric](https://learn.microsoft.com/en-us/fabric/security/security-overview)
- [Permission Model](https://learn.microsoft.com/en-us/fabric/security/permission-model)
- [OneLake data security overview](https://learn.microsoft.com/en-us/fabric/onelake/security/get-started-security)
- [OneLake data access control model](https://learn.microsoft.com/en-us/fabric/onelake/security/data-access-control-model)
- [Conditional Access](https://learn.microsoft.com/en-us/fabric/security/security-conditional-access)
- [Private Links](https://learn.microsoft.com/en-us/fabric/security/security-private-links-overview)
- [Managed VNets](https://learn.microsoft.com/en-us/fabric/security/security-managed-vnets-fabric-overview)
