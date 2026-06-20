# Azure Governance, Policy & Cost Control Lab

> **Disclaimer:** All Azure resources created in this lab were deleted after completion. No services were left running. This lab was built purely for learning and documentation purposes.

---

## Description

This lab focuses on building a governance framework in Azure — covering Management Groups, Azure Policy definitions (tag enforcement, VM SKU restrictions, region locking), resource locks, and Cost Management budgets with alerting thresholds.

| Detail                  | Value                          |
| ----------------------- | ------------------------------ |
| **Resource Group**      | `Ropu-Iwi`               |
| **Region**              | `Australia East`                |
| **Management Group**    | `Kawana Mg`               |
| **Policy Definitions**  | Tag Enforcement / VM Allow SKU / Region Lock |
| **Resource Locks**      | ReadOnly / CanNotDelete        |
| **Budget Name**         | `Matini-Lab-Budget`           |
| **Budget Amount**       | `[10$]`                   |

---

## What I Covered in This Lab

- Created a Management Group for subscriptions, however it was not used due to licensing 
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

I created a Management Group above the subscription to enable governance controls at scale. Due to subscription limitations, I used the Root Management Group instead to apply and demonstrate the required controls.

| Detail              | Value                      |
| ------------------- | -------------------------- |
| **MG Name**         | `Kawana Mg`           |
| **MG ID**           | `Mg-Kawana-lab`             |
| **Subscription**    | `Free Subscription`  |

**Steps:**

1. Navigated to **Management group** → **+ Create**
2. Name: **Kawana Mg** 
3. Needed to be assigned as Parent management group 

<img src="https://imgur.com/93hwiU4.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/Op0jblE.png" height="80%" width="80%" alt=""/>

---

## Part 2 — Azure Policy

I wrote and assigned three policy definitions scoped to the Management Group, enforcing tagging, restricting VM SKUs, and locking deployments to approved regions.

**Tag Enforcement Policy**

This policy required a specified tag (e.g. `Environment`) to be present on all resources. Resources missing the tag were denied at deployment time.

| Detail           | Value                          |
| ---------------- | ------------------------------ |
| **Policy Name**  | `Require a tag on resource`    |
| **Effect**       | Deny                           |
| **Tag Required** | `Environment`                  |

<img src="https://imgur.com/DK6Nz5l.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/wspwaUR.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/emuQA8d.png" height="80%" width="80%" alt=""/>

---

**VM** SKU Restriction Policy

This policy restricted which VM sizes could be deployed within the subscription, preventing use of expensive or unapproved SKUs.

| Detail              | Value                          |
| ------------------- | ------------------------------ |
| **Policy Name**     | `Allowed Virtual Machine size SKUs`           |
| **Effect**          | Deny                           |
| **Allowed SKUs**    | `Standard_B2s` |

<img src="https://imgur.com/hi5UgJI.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/b1NHYMr.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/r0vGHZy.png" height="80%" width="80%" alt=""/>

---

### 2c — Region Lock Policy

This policy denied deployments to any region outside of an approved list, ensuring resources stayed within the designated geography.

| Detail              | Value                              |
| ------------------- | ---------------------------------- |
| **Policy Name**     | `Allowed Location`               |
| **Effect**          | Deny                               |
| **Allowed Regions** | `australiaeast, australiasoutheast` |

<img src="https://imgur.com/QexasyM.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/yIkExFH.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/QLLRFHi.png" height="80%" width="80%" alt=""/>

---

## Part 3 — Resource Locks

I applied resource locks to protect critical resources from accidental deletion or modification.

| Resource           | Lock Type     | Purpose                                      |
| ------------------ | ------------- | -------------------------------------------- |
| `Ropu-Iwi`  | CanNotDelete  | Prevents deletion while allowing edits       |
| `Papatuanuku`  | ReadOnly      | Prevents all modifications including deletes |

<img src="https://imgur.com/ojeMnMG.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/bsz7eIF.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/jXbeSsB.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/JQb3C7b.png" height="80%" width="80%" alt=""/>

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

<img src="https://imgur.com/d85L2md.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/t8cgj9P.png" height="80%" width="80%" alt=""/>

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
