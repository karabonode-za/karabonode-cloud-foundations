# Lab 01 · Take a tour of Azure

> ☁️ Cloud Foundations · **Lab 1 of 6** · ⏱ ~1 hour · 💰 Free · 🎯 AZ-900: Azure architecture

---

## The story

It's your first day at **Mzansi Threads**. Lerato hands you the login to a brand-new Azure subscription and says: *"Everything we build will live in here. Find your way around first."*

Before you build anything, you need to know **where things go** and **how Azure is organised**. That's this lab.

## What you'll learn

- What a **subscription**, **resource group** and **region** are
- How Azure is organised, from management groups down to individual resources
- How to use **tags** to label things (so you know who owns what, and what it costs)
- How to open **Cloud Shell**, a terminal in your browser

## Before you start

- [ ] You can sign in at [portal.azure.com](https://portal.azure.com)

## Steps

### 1. Look around the portal

Sign in to the Azure portal. Find:
- The **search bar** at the top (the fastest way to find anything)
- **Subscriptions**: click it and note your subscription's name
- **All resources**: probably empty. That's about to change.

> 💡 **A subscription** is like an account with Azure: it's where resources live and where the bill goes.

✅ **Done when:** you can find your subscription name.

### 2. Understand how Azure is organised

Azure has four levels. Think of it like a company filing system:

```
Management groups   ← folders for many subscriptions (big companies)
  └── Subscription  ← the "account" and the bill
        └── Resource group   ← a project folder
              └── Resources  ← the actual things: storage, VMs, websites
```

✍️ In your notes, write one sentence about each level in your own words.

### 3. Create your first resource group

A **resource group** is a folder for everything that belongs to one project. Delete the folder, and everything inside goes with it.

1. Search **Resource groups** → **+ Create**
2. Name: `rg-mzansi-learn`
3. Region: **South Africa North** (Johannesburg)
4. Click **Next: Tags** and add:
   - `owner` = your name
   - `project` = `mzansi-threads`
   - `environment` = `learning`
5. **Review + create** → **Create**

✅ **Done when:** `rg-mzansi-learn` appears in your list of resource groups, with three tags.

### 4. Explore regions

**Regions** are groups of data centres in a place. Azure has two in South Africa.

1. Open **Cloud Shell** (the `>_` icon at the top of the portal). Choose **Bash**. If it asks about storage, pick the option to continue **without** a storage account.
2. Run:
   ```bash
   az account list-locations --query "[?contains(displayName, 'South Africa')].{Name:name, Display:displayName}" -o table
   ```

✍️ Write down both South African regions. Why might Mzansi Threads want its data in South Africa? (Hint: speed for local customers, and POPIA privacy rules.)

✅ **Done when:** you've seen both regions in Cloud Shell.

### 5. Do the same thing with a command

Everything you click in the portal can also be done with a command. Create a second resource group from Cloud Shell:

```bash
az group create --name rg-mzansi-cli --location southafricanorth --tags owner=me project=mzansi-threads
az group list -o table
```

✅ **Done when:** both resource groups appear in the table.

## Check yourself

<details><summary>1. What happens to the resources inside a resource group when you delete the group?</summary>
They're all deleted too. That's why one resource group per project makes clean-up easy.
</details>

<details><summary>2. Why use tags?</summary>
To label resources with owner, project, environment and so on, so you can find them, and see costs per project.
</details>

<details><summary>3. What's the difference between a region and a resource group?</summary>
A region is a <em>physical place</em> (data centres). A resource group is a <em>logical folder</em> for organising your work.
</details>

## Clean-up

Delete the CLI group (keep `rg-mzansi-learn` for the next labs):

```bash
az group delete --name rg-mzansi-cli --yes
```

## Save your evidence

In `my-cloud-foundations/01-azure-tour/notes.md`:
- Your one-line explanations of management group / subscription / resource group / resource
- A screenshot of your resource group with its tags (hide your subscription ID)

---

**Next →** [Lab 02 · Control costs first](../02-cost-control/)
