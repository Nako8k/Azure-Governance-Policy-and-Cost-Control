# Azure Governance, Policy & Cost Control Lab

> **Disclaimer:** All Azure resources created in this lab were deleted after completion. No services were left running. This lab was built purely for learning and documentation purposes.

---

<img src="https://imgur.com/34pfAAg.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/93hwiU4.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/Op0jblE.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/DK6Nz5l.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/wspwaUR.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/emuQA8d.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/MqJZLAI.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/6hHsBx5.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/hi5UgJI.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/b1NHYMr.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/r0vGHZy.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/QexasyM.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/yIkExFH.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/Gd1ClVz.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/XwVIwxf.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/QLLRFHi.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/ojeMnMG.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/bsz7eIF.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/jXbeSsB.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/JQb3C7b.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/d85L2md.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/t8cgj9P.png" height="80%" width="80%" alt=""/>




## Description

This lab focuses on building a governance framework in Azure — covering Management Groups, Azure Policy definitions (tag enforcement, VM SKU restrictions, region locking), resource locks, and Cost Management budgets with alerting thresholds.

| Detail                  | Value                          |
| ----------------------- | ------------------------------ |
| **Subscription**        | `[Your Subscription Name]`     |
| **Resource Group**      | `[Your RG Name]`               |
| **Region**              | `[Your Region]`                |
| **Management Group**    | `[Your MG Name]`               |
| **Policy Definitions**  | Tag Enforcement / VM SKU / Region Lock |
| **Resource Locks**      | ReadOnly / CanNotDelete        |
| **Budget Name**         | `[Your Budget Name]`           |
| **Budget Amount**       | `[$ Amount]`                   |

---

## What I Covered in This Lab

- Created a Management Group hierarchy and moved a subscription into it
- Wrote and assigned custom Azure Policy definitions for tag enforcement, VM SKU restrictions, and region locking
- Applied ReadOnly and CanNotDelete resource locks to critical resources
- Set up a Cost Management budget with alert thresholds at 50%, 80%, and 100%
- Documented the governance model with consistent resource tagging across all resources

---

## Lab Structure

```
azure-governance-lab/
├── README.md
└── screenshots/
    ├── part1-management-groups/
    ├── part2-azure-policy/
    ├── part3-resource-locks/
    ├── part4-cost-management/
    └── part5-tagging/
```

---

## Part 1 — Management Groups

I created a Management Group to sit above the subscription, which allowed me to apply governance controls at scale rather than per-subscription.

| Detail              | Value                      |
| ------------------- | -------------------------- |
| **MG Name**         | `[Your MG Name]`           |
| **MG ID**           | `[Your MG ID]`             |
| **Subscription**    | `[Subscription moved in]`  |

**Steps:**

1. <!-- [What you did — e.g. Navigate to Management Groups → + Create] -->
2. <!-- [Step 2] -->
3. <!-- [Step 3] -->

> <!-- [Any gotchas or notes — e.g. It can take a few minutes for the subscription move to propagate] -->

<!-- [Screenshot placeholder — replace with your imgur/hosted image link] -->
<!-- ![Management Group](https://imgur.com/your-image.png) -->

---

## Part 2 — Azure Policy

I wrote and assigned three policy definitions scoped to the Management Group, enforcing tagging, restricting VM SKUs, and locking deployments to approved regions.

### 2a — Tag Enforcement Policy

This policy required a specified tag (e.g. `Environment`) to be present on all resources. Resources missing the tag were denied at deployment time.

| Detail           | Value                          |
| ---------------- | ------------------------------ |
| **Policy Name**  | `[Your Policy Name]`           |
| **Effect**       | Deny                           |
| **Tag Required** | `[e.g. Environment]`           |
| **Scope**        | `[MG or Subscription]`         |

**Steps:**

1. <!-- [e.g. Navigate to Policy → Definitions → + Policy definition] -->
2. <!-- [Step 2 — writing the policy rule JSON] -->
3. <!-- [Step 3 — assigning the policy] -->

> <!-- [Note — e.g. Deny effect blocks resource creation immediately; Audit only logs violations without blocking] -->

<!-- ![Tag Policy](https://imgur.com/your-image.png) -->

---

### 2b — VM SKU Restriction Policy

This policy restricted which VM sizes could be deployed within the subscription, preventing use of expensive or unapproved SKUs.

| Detail              | Value                          |
| ------------------- | ------------------------------ |
| **Policy Name**     | `[Your Policy Name]`           |
| **Effect**          | Deny                           |
| **Allowed SKUs**    | `[e.g. Standard_B1s, Standard_B2s]` |
| **Scope**           | `[MG or Subscription]`         |

**Steps:**

1. <!-- [Step 1] -->
2. <!-- [Step 2] -->
3. <!-- [Step 3] -->

> <!-- [Note — e.g. Used the built-in 'Allowed virtual machine size SKUs' policy definition rather than writing custom JSON] -->

<!-- ![VM SKU Policy](https://imgur.com/your-image.png) -->

---

### 2c — Region Lock Policy

This policy denied deployments to any region outside of an approved list, ensuring resources stayed within the designated geography.

| Detail              | Value                              |
| ------------------- | ---------------------------------- |
| **Policy Name**     | `[Your Policy Name]`               |
| **Effect**          | Deny                               |
| **Allowed Regions** | `[e.g. australiaeast, australiasoutheast]` |
| **Scope**           | `[MG or Subscription]`             |

**Steps:**

1. <!-- [Step 1] -->
2. <!-- [Step 2] -->
3. <!-- [Step 3] -->

> <!-- [Note — e.g. The 'Allowed locations' built-in policy was used here; global resources like Azure AD are excluded by default] -->

<!-- ![Region Lock Policy](https://imgur.com/your-image.png) -->

---

## Part 3 — Resource Locks

I applied resource locks to protect critical resources from accidental deletion or modification.

| Resource           | Lock Type     | Purpose                                      |
| ------------------ | ------------- | -------------------------------------------- |
| `[Resource Name]`  | CanNotDelete  | Prevents deletion while allowing edits       |
| `[Resource Name]`  | ReadOnly      | Prevents all modifications including deletes |

**Steps:**

1. <!-- [e.g. Navigate to the resource → Settings → Locks → + Add] -->
2. <!-- [Step 2 — applying CanNotDelete lock] -->
3. <!-- [Step 3 — applying ReadOnly lock] -->
4. <!-- [Step 4 — testing the lock by attempting a deletion or change] -->

> <!-- [Note — e.g. ReadOnly locks on a Resource Group block the creation of new resources inside it, not just edits to the RG itself — this caught me off guard] -->

<!-- ![Resource Locks](https://imgur.com/your-image.png) -->

---

## Part 4 — Cost Management Budget

I created a budget in Azure Cost Management with three alert thresholds to track spending and get notified before hitting limits.

| Detail              | Value                        |
| ------------------- | ---------------------------- |
| **Budget Name**     | `[Your Budget Name]`         |
| **Amount**          | `$[Amount]`                  |
| **Reset Period**    | `[Monthly / Quarterly]`      |
| **Alert at 50%**    | Notification to `[email]`    |
| **Alert at 80%**    | Notification to `[email]`    |
| **Alert at 100%**   | Notification to `[email]`    |

**Steps:**

1. <!-- [e.g. Navigate to Cost Management + Billing → Budgets → + Add] -->
2. <!-- [Step 2 — setting the budget scope, amount, and reset period] -->
3. <!-- [Step 3 — adding the 50% alert condition] -->
4. <!-- [Step 4 — adding the 80% alert condition] -->
5. <!-- [Step 5 — adding the 100% alert condition] -->
6. <!-- [Step 6 — adding alert recipients and saving] -->

> <!-- [Note — e.g. Alerts are email-based and are not real-time — Azure evaluates spending once every 24 hours, so you may not be notified immediately when a threshold is crossed] -->

<!-- ![Budget Overview](https://imgur.com/your-image.png) -->

<!-- ![Budget Alerts](https://imgur.com/your-image.png) -->

---

## Part 5 — Resource Tagging Strategy

I applied a consistent tagging strategy across all resources to support cost allocation, governance, and policy compliance.

| Tag Key       | Tag Value Example        | Purpose                            |
| ------------- | ------------------------ | ---------------------------------- |
| `Environment` | `[e.g. Dev / Prod]`      | Identifies the environment tier    |
| `Owner`       | `[Your name or alias]`   | Tracks resource ownership          |
| `Project`     | `[Lab or project name]`  | Groups resources by project        |
| `CostCentre`  | `[e.g. Training]`        | Supports cost allocation reporting |

> <!-- [Note — e.g. The tag enforcement policy from Part 2 automatically blocked any resource creation that was missing the Environment tag] -->

<!-- ![Tagging](https://imgur.com/your-image.png) -->

---

## Key Takeaways

- <!-- [e.g. Management Groups let you apply governance controls once across multiple subscriptions] -->
- <!-- [e.g. Policy 'Deny' effects are powerful — test with 'Audit' first before switching to Deny in production] -->
- <!-- [e.g. Resource locks exist independently of RBAC — even an Owner cannot delete a CanNotDelete-locked resource without removing the lock first] -->
- <!-- [e.g. Budget alerts are informational only — Azure does not automatically stop spending when a budget is exceeded] -->
- <!-- [Add your own takeaway] -->

---

## Lessons Learned

- <!-- [e.g. I initially scoped the policy assignment to the subscription rather than the Management Group — this meant the policy wouldn't apply to future subscriptions added under the MG, which defeated the purpose of the hierarchy] -->
- <!-- [e.g. ReadOnly locks surprised me by blocking resource creation inside a locked Resource Group, not just edits to the RG itself] -->
- <!-- [e.g. Add your own lesson] -->

---

*Lab completed by [@Nako8k](https://github.com/Nako8k)*
