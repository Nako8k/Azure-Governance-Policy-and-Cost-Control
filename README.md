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

I created a Management Group above the subscription to enable governance controls at scale. Due to subscription limitations, I used the **Root Management Group** instead to apply and demonstrate the required controls.

| Detail              | Value                      |
| ------------------- | -------------------------- |
| **MG Name**         | `Kawana Mg`           |
| **MG ID**           | `Mg-Kawana-lab`             |
| **Subscription**    | `Free Subscription`  |

**Steps:**

1. Navigated to **Management group** → **+ Create**
2. Name: **Kawana Mg** 
3. Assign as Parent management group 

<img src="https://imgur.com/93hwiU4.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/Op0jblE.png" height="80%" width="80%" alt=""/>

---

## Part 2 — Azure Policy

I wrote and assigned three policy definitions scoped to the Management Group, enforcing tagging, restricting VM SKUs, and locking deployments to approved regions.

**Tag Enforcement Policy**

This policy required a specified tag `Environment` to be present on all resources. Resources missing the tag were denied at deployment time.

| Detail           | Value                          |
| ---------------- | ------------------------------ |
| **Policy Name**  | `Require a tag on resource`    |
| **Effect**       | Deny                           |
| **Tag Required** | `Environment`                  |

<img src="https://imgur.com/DK6Nz5l.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/wspwaUR.png" height="80%" width="80%" alt=""/>

Here are the results of non-compliance with the policy.

<img src="https://imgur.com/emuQA8d.png" height="80%" width="80%" alt=""/>

---

**VM SKU Restriction Policy**

This policy restricted which VM sizes could be deployed within the subscription, preventing use of expensive or unapproved SKUs.

| Detail              | Value                          |
| ------------------- | ------------------------------ |
| **Policy Name**     | `Allowed Virtual Machine size SKUs`           |
| **Effect**          | Deny                           |
| **Allowed SKUs**    | `Standard_B2s` |

<img src="https://imgur.com/hi5UgJI.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/b1NHYMr.png" height="80%" width="80%" alt=""/>

Same as before the results of non-compliance with the policy `Allowed Virtual Machine size SKUs` as well as `Require a tag on resource`.

<img src="https://imgur.com/r0vGHZy.png" height="80%" width="80%" alt=""/>

---

**Region Lock Policy** 

This policy denied deployments to any region outside of an approved list, ensuring resources stayed within the designated geography.

| Detail              | Value                              |
| ------------------- | ---------------------------------- |
| **Policy Name**     | `Allowed Location`               |
| **Effect**          | Deny                               |
| **Allowed Regions** | `australiaeast, australiasoutheast` |

<img src="https://imgur.com/QexasyM.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/yIkExFH.png" height="80%" width="80%" alt=""/>

This message is displayed when the resource is not located in either the `australiaeast` or `australiasoutheast` regions.

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

Effects of attempting to modify resource locks

<img src="https://imgur.com/jXbeSsB.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/JQb3C7b.png" height="80%" width="80%" alt=""/>

---

## Part 4 — Cost Management Budget

I created a budget in Azure Cost Management with three alert thresholds to track spending and get notified before hitting limits.

| Detail              | Value                        |
| ------------------- | ---------------------------- |
| **Budget Name**     | `Matatin-Lab-Budget`         |
| **Amount**          | `$10`                        |
| **Reset Period**    | `Monthly`                    |
| **Alert at 50%**    | Notification to `My email`   |
| **Alert at 80%**    | Notification to `My email`   |
| **Alert at 100%**   | Notification to `My email`   |

**Steps:**

1. Navigate to Cost Management + Billing → Budgets → + Add
2. I then configured the budget, scope, reset period, and alerts
3. I added the 50% alert condition, 80% alert condition and  100% alert condition
4. completed by adding the alert recipients email

<img src="https://imgur.com/d85L2md.png" height="80%" width="80%" alt=""/>

<img src="https://imgur.com/t8cgj9P.png" height="80%" width="80%" alt=""/>

---

## Key Takeaways

- Management Groups let me apply governance controls once across multiple subscriptions
- Policy 'Deny' effects are powerful — test with 'Audit' first before switching to Deny in production
- Resource locks exist independently of RBAC — even an Owner cannot delete a CanNotDelete resource without removing the lock first
- Budget alerts are informational only — Azure does not automatically stop spending when a budget is exceeded

---

## Lessons Learned

- I initially scoped the policy assignment to the subscription rather than the Management Group — this meant the policy wouldn't apply to future subscriptions added under the MG, this section will be properly completed for future labs and projects.
- Applying locks to resources and testing the outcomes became good practice and helped me understand the importance of having different policies running on resources in a tenant, both as a security measure and as an accidental protective measure.
---

*Lab completed by [@Nako8k](https://github.com/Nako8k)*
