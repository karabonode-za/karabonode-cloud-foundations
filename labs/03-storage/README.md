# Lab 03 · Store files in the cloud

> ☁️ Cloud Foundations · **Lab 3 of 6** · ⏱ ~1 hour · 💰 Cents · 🎯 AZ-900: Azure storage

---

## The story

Mzansi Threads' product photos live on Lerato's phone and a USB stick. If the phone gets stolen, they're gone. She also needs to send a photo to a printing company **without making all her photos public**.

Your job: move the photos into **Azure Storage**, and share **one** photo safely.

## What you'll learn

- What a **storage account** is, and what **blob storage** is for
- **Redundancy** (how many copies Azure keeps) and **access tiers** (hot vs cool vs archive)
- Keeping files **private**, and sharing one file with a time-limited link (**SAS**)

## Before you start

- [ ] Labs 01–02 done (resource group + budget)
- [ ] 2–3 photos on your computer (any pictures of clothes, or any images)

## Steps

### 1. Create a storage account

1. Search **Storage accounts** → **+ Create**
2. Resource group: `rg-mzansi-learn`
3. Name: `stmzansi` + your initials + numbers, e.g. `stmzansilm2026` (lowercase letters and numbers only, must be unique in all of Azure)
4. Region: **South Africa North** · Primary service: **Azure Blob Storage** · Performance: **Standard**
5. Redundancy: **LRS** (locally redundant, the cheapest)
6. **Review + create** → **Create**

> 💡 **Redundancy** = how many copies Azure keeps, and where. **LRS** keeps 3 copies in one data centre. **GRS** also copies to another region, which survives a whole-region disaster but costs more.

✅ **Done when:** the storage account is deployed.

### 2. Create a private container and upload photos

1. Open your storage account → **Data storage → Containers** → **+ Container**
2. Name: `product-photos` · Anonymous access level: **Private**
3. Open the container → **Upload** → choose your photos

Now try it: open a photo → copy its **URL** → paste it into a private browser window.

❌ You get an error. **Good.** Private means private.

✅ **Done when:** your photos are uploaded and the direct URL doesn't work for strangers.

### 3. Share one photo safely with a SAS link

The printing company needs **one** photo, for **one day**.

1. Click one photo → **Generate SAS**
2. Permissions: **Read** only · Expiry: **tomorrow**
3. **Generate SAS token and URL** → copy the **Blob SAS URL**
4. Paste it into a private browser window

✅ It opens! Only *this* photo, only until tomorrow.

> 🔒 A SAS link is like a temporary key. Anyone who has it can use it until it expires, so keep expiry times short.

### 4. Explore access tiers

Open a photo → **Change tier**. You'll see **Hot**, **Cool**, **Cold** and **Archive**.

✍️ In your notes: *Which tier for photos on the website (viewed every day)? Which for last year's photos Lerato must keep but never looks at?*

## Check yourself

<details><summary>1. What does LRS protect you from, and what doesn't it?</summary>
LRS protects against a disk or server failing (3 copies in one data centre). It doesn't protect against the whole data centre or region going down; GRS does.
</details>

<details><summary>2. Why not just make the container public?</summary>
Then <em>every</em> file is readable by anyone with the URL, forever. A SAS link shares one file, with limited permissions, for a limited time.
</details>

<details><summary>3. When would you use the Archive tier?</summary>
For data you must keep but almost never read. It's the cheapest to store but slow and costly to read back.
</details>

## Clean-up

Keep the storage account. You'll use it again in the capstone. (Storing a few photos costs cents.)

## Save your evidence

`my-cloud-foundations/03-storage/notes.md`: your tier answers, and a screenshot of the "access denied" error next to the working SAS link (hide the SAS token part of the URL!).

---

**← Previous:** [Lab 02](../02-cost-control/) · **Next →** [Lab 04 · Your first virtual machine](../04-virtual-machines-networking/)
