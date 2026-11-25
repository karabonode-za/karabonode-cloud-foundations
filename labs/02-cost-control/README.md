# Lab 02 · Control costs first

> ☁️ Cloud Foundations · **Lab 2 of 6** · ⏱ ~1 hour · 💰 Free · 🎯 AZ-900: Cost management

---

## The story

Lerato's biggest fear: *"I've heard stories of people getting a huge cloud bill they didn't expect."*

She's right to worry. In the cloud, **you pay for what you use**, and forgetting to switch something off costs real money. Professionals set up cost controls **before** they build. So that's what you'll do.

## What you'll learn

- How cloud pricing works (**pay-as-you-go**, and what "consumption-based" means)
- How to set a **budget** that emails you before you overspend
- How to estimate a cost **before** building, with the **Pricing Calculator**
- Where to see what you've spent (**Cost analysis**)

## Before you start

- [ ] Lab 01 done (you have `rg-mzansi-learn`)

## Steps

### 1. Set a budget alert

1. Search **Cost Management** → **Budgets** → **+ Add**
2. Scope: your subscription
3. Name: `budget-mzansi-monthly` · Reset period: **Monthly** · Amount: a small number, e.g. **200** (in your billing currency)
4. **Next** → add alert conditions:
   - **50%** of budget (Actual)
   - **90%** of budget (Actual)
   - **100%** of budget (Forecasted): warns you *before* you get there
5. Alert recipients: your email → **Create**

> ⚠️ A budget **sends an email**. It doesn't switch anything off. You still have to act on it.

✅ **Done when:** your budget appears in the list.

### 2. Estimate before you build

Lerato wants to know what a small website and photo storage would cost.

1. Open the [Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator/)
2. Add **Storage Accounts**: Region *South Africa North*, Type *Block Blob Storage*, Redundancy *LRS*, Access tier *Hot*, capacity **5 GB**
3. Add **Virtual Machines**: Region *South Africa North*, size **B1s**, Linux, **730 hours** (a full month)
4. Change the VM to **100 hours** and watch the price change

✍️ In your notes: *How much would storage cost per month? A VM running all month? Only 100 hours? What does that tell you about switching things off?*

✅ **Done when:** you have three numbers written down.

### 3. Explore Cost analysis

Search **Cost Management** → **Cost analysis**. Your spend is probably close to zero right now. Look at:
- **Group by** → *Resource group*
- **Group by** → *Tag* → `project` (this is why you added tags in Lab 01!)

✅ **Done when:** you can find where costs would show up per resource group and per tag.

### 4. Check Azure Advisor

Search **Advisor** → **Cost** tab. Advisor gives free recommendations like "this VM is barely used, make it smaller". It may be empty now; come back after Lab 04.

## Check yourself

<details><summary>1. Does a budget stop resources when you reach the limit?</summary>
No. It only sends alerts. You need to act, e.g. stop or delete resources.
</details>

<details><summary>2. Why does a VM cost less if it only runs 100 hours?</summary>
Cloud compute is pay-as-you-go: you're billed for the time it runs. A stopped (deallocated) VM doesn't pay for compute, though its disk still costs a little.
</details>

<details><summary>3. How do tags help with costs?</summary>
You can group and filter costs by tag, e.g. see exactly what the <code>mzansi-threads</code> project costs.
</details>

## Clean-up

Nothing to delete. **Keep the budget**: it protects you for the rest of the series.

## Save your evidence

`my-cloud-foundations/02-cost-control/notes.md`: your three cost estimates, and one sentence on how you'd keep Mzansi Threads' bill low.

---

**← Previous:** [Lab 01](../01-azure-tour/) · **Next →** [Lab 03 · Store files in the cloud](../03-storage/)
