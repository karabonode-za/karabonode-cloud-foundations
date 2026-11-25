# Lab 05 · Who can do what

> ☁️ Cloud Foundations · **Lab 5 of 6** · ⏱ ~1 hour · 💰 Free · 🎯 AZ-900: Identity, access & governance

---

## The story

Lerato has hired a freelance web designer, **Bongani**, for two weeks. He needs to **see** the Azure setup, but he must **not** be able to change or delete anything. Lerato also wants a rule: *"Our data stays in South Africa. Always."*

You'll set up **who can do what** (access) and **rules Azure enforces automatically** (governance).

## What you'll learn

- **Microsoft Entra ID**: where users live
- **Azure RBAC**: giving someone only the access they need (**least privilege**)
- **Azure Policy**: rules that block things you don't allow
- **Resource locks**: protection against accidental deletion

## Before you start

- [ ] Labs 01–04 done (`rg-mzansi-learn` with your storage account)
- [ ] You're the admin of your subscription (true for personal / student accounts)

## Steps

### 1. Create a user for the contractor

1. Search **Microsoft Entra ID** → **Users** → **+ New user** → **Create new user**
2. User principal name: `bongani` · Display name: `Bongani (Contractor)`
3. Tick **Auto-generate password** and copy it → **Create**

✅ **Done when:** Bongani appears in the user list.

### 2. Give read-only access (least privilege)

1. Open `rg-mzansi-learn` → **Access control (IAM)** → **+ Add** → **Add role assignment**
2. Role: **Reader** → **Next**
3. Members: **+ Select members** → Bongani → **Review + assign**

> 💡 **Least privilege** = give people only the access they need, for only as long as they need it. Reader can look but not touch.

### 3. Test it as Bongani

1. Open a **private/incognito** browser window → [portal.azure.com](https://portal.azure.com)
2. Sign in as Bongani (`bongani@<your-domain>.onmicrosoft.com`) with the temporary password, and set a new one when asked
3. Open `rg-mzansi-learn`: he can **see** it ✅
4. Try to **delete** the storage account, or create something ❌ **Access denied**

✅ **Done when:** you've seen Bongani get "access denied".

### 4. Enforce "data stays in South Africa" with Azure Policy

1. Search **Policy** → **Assignments** → **Assign policy**
2. Scope: your subscription → resource group `rg-mzansi-learn`
3. Policy definition: search **Allowed locations** → select it
4. **Parameters** tab → Allowed locations: **South Africa North** and **South Africa West**
5. **Review + create** → **Create**

Wait ~5 minutes, then try to create a storage account in `rg-mzansi-learn` in **West Europe**.

❌ **Denied by policy.** Azure enforces the rule for you, every time.

### 5. Lock it against accidental deletion

1. Open `rg-mzansi-learn` → **Locks** → **+ Add**
2. Name: `no-delete` · Lock type: **Delete** → **OK**
3. Try to delete the storage account ❌ **Blocked by lock**

✍️ Even an *owner* can't delete it until the lock is removed. When would that save a business?

## Check yourself

<details><summary>1. What's the difference between Entra ID and Azure RBAC?</summary>
Entra ID is <em>who you are</em> (the user account and sign-in). Azure RBAC is <em>what you're allowed to do</em> on Azure resources (e.g. Reader on a resource group).
</details>

<details><summary>2. What's the difference between RBAC and Azure Policy?</summary>
RBAC controls what <em>people</em> can do. Policy controls what <em>resources</em> are allowed to look like (e.g. which regions), no matter who creates them.
</details>

<details><summary>3. Bongani's contract ends. What should you do?</summary>
Remove his role assignment and disable or delete his account. Access should end when the need ends.
</details>

## Clean-up

- **Remove the lock** (Locks → delete `no-delete`). You'll add it again properly in the capstone.
- Remove Bongani's **Reader** role assignment, then **delete** his user account.
- Keep the policy assignment and the storage account for the capstone.

## Save your evidence

`my-cloud-foundations/05-access/notes.md`: screenshots of "access denied" (as Bongani), "denied by policy" and "blocked by lock", plus one sentence on least privilege.

---

**← Previous:** [Lab 04](../04-virtual-machines-networking/) · **Next →** [Lab 06 · 🏁 Capstone: Mzansi Threads online](../06-capstone-mzansi-threads-online/)
