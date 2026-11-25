# Lab 06 · 🏁 Capstone: Mzansi Threads online

> ☁️ Cloud Foundations · **Lab 6 of 6** · ⏱ ~3 hours · 💰 Cents · 🎯 AZ-900: all areas

---

## The story

It's launch week. Lerato wants **Mzansi Threads online**: a simple website with their product photos that customers can visit, that costs almost nothing, that stays in South Africa, and that a contractor can review without breaking anything.

You've learned every building block. Now you'll **put them together** and hand Lerato a working website **plus** a short document explaining what you built and why. That document is what makes this a portfolio project.

## What you'll build

```
Customer's browser ──► Azure Storage static website ($web)  ──  South Africa North
                         │
   Inside resource group rg-mzansi-prod (tagged, budgeted, policy: SA regions only, delete lock)
                         │
   Bongani (Reader) ─────┘  can view, can't change
```

| Building block | From lab |
|---|---|
| Resource group + tags | 01 |
| Budget alert + cost estimate | 02 |
| Storage account | 03 |
| Choosing PaaS over a VM | 04 |
| Reader role, policy, lock | 05 |

## Before you start

- [ ] Labs 01–05 done
- [ ] 2–3 product photos (any images)

## Steps

### 1. Set up the production resource group

Create `rg-mzansi-prod` in **South Africa North** with tags `owner`, `project=mzansi-threads` and `environment=production`.

Assign the **Allowed locations** policy (South Africa North + West) to it, exactly like Lab 05.

✅ **Done when:** the group exists with 3 tags and the policy assigned.

### 2. Estimate the cost first

Using the Pricing Calculator (Lab 02), estimate a storage account with **1 GB**, Hot tier, LRS in South Africa North. Write the monthly number down. It goes in your write-up.

### 3. Create the storage account and turn on static website

Open **Cloud Shell (Bash)**. Choose a unique name (lowercase letters and numbers):

```bash
ST=stmzansiweb$RANDOM
az storage account create -n $ST -g rg-mzansi-prod -l southafricanorth \
  --sku Standard_LRS --kind StorageV2 --tags project=mzansi-threads environment=production

az storage blob service-properties update --account-name $ST --auth-mode login \
  --static-website --index-document index.html --404-document 404.html
```

> 💡 **Static website** = Azure serves your HTML files directly. No server to patch, nothing to keep running. This is **PaaS**, the decision you prepared for in Lab 04.

> If you get a permissions error with `--auth-mode login`, give yourself the **Storage Blob Data Contributor** role on the storage account (Access control (IAM)), wait a minute and try again.

### 4. Build the website

In Cloud Shell, create a folder with the site:

```bash
mkdir -p site/images && cd site
cat > index.html <<'EOF'
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Mzansi Threads</title>
  <style>
    body { font-family: system-ui, sans-serif; margin: 0; background: #faf7f2; color: #222; }
    header { background: #111; color: #fff; padding: 2rem; text-align: center; }
    .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 1rem; padding: 2rem; }
    .card { background: #fff; border-radius: 12px; padding: 1rem; box-shadow: 0 2px 8px #0001; }
    .card img { width: 100%; border-radius: 8px; }
  </style>
</head>
<body>
  <header><h1>Mzansi Threads</h1><p>Made in Soweto. Worn everywhere.</p></header>
  <main class="grid">
    <div class="card"><img src="images/item1.jpg" alt="Product 1"><h2>Kasi Hoodie</h2><p>R450</p></div>
    <div class="card"><img src="images/item2.jpg" alt="Product 2"><h2>Vilakazi Tee</h2><p>R250</p></div>
  </main>
  <footer style="text-align:center;padding:1rem">Order on WhatsApp</footer>
</body>
</html>
EOF
echo '<h1>Page not found</h1><a href="/">Back to Mzansi Threads</a>' > 404.html
```

Upload two photos to Cloud Shell (the **Upload** button in the Cloud Shell toolbar), then move them in and name them `item1.jpg` and `item2.jpg`:

```bash
mv ~/your-photo-1.jpg images/item1.jpg
mv ~/your-photo-2.jpg images/item2.jpg
```

### 5. Publish it

```bash
az storage blob upload-batch --account-name $ST --auth-mode login -s . -d '$web' --overwrite
az storage account show -n $ST -g rg-mzansi-prod --query "primaryEndpoints.web" -o tsv
```

Open the URL. 🎉 **Mzansi Threads is online.**

✅ **Done when:** the site loads on your phone too.

### 6. Protect it

Apply what you learned in Lab 05:

1. Add Bongani again (or a new test user) as **Reader** on `rg-mzansi-prod`, and test that he can view but not change anything.
2. Add a **Delete** lock to `rg-mzansi-prod`.
3. Check your **budget** from Lab 02 covers this subscription.

✅ **Done when:** Reader access, lock and budget are all in place.

### 7. Write the handover document

Create `my-cloud-foundations/06-capstone/README.md` with:

1. **What I built**: one paragraph and the live URL
2. **Architecture diagram**: draw it in [draw.io](https://app.diagrams.net) or [Excalidraw](https://excalidraw.com) and add the PNG
3. **Why a static website, not a VM**: use your Lab 04 table
4. **Cost**: your estimate from step 2 and how the budget protects Lerato
5. **Security & governance**: Reader access, policy, lock, and why each matters
6. **What I'd do next**: e.g. custom domain, HTTPS with Azure Front Door, a shop backend

## Check yourself

<details><summary>1. Lerato asks: "What happens if someone deletes the website by accident?" What do you answer?</summary>
The resource group has a Delete lock, so it can't be deleted until an owner removes the lock on purpose. The site files are also in your own folder and can be re-uploaded in one command.
</details>

<details><summary>2. Why is this cheaper than the VM from Lab 04?</summary>
You only pay for a tiny amount of storage and traffic. There's no computer running 24/7, and no OS to patch.
</details>

<details><summary>3. Which parts of the shared responsibility model are now Azure's job?</summary>
The servers, operating system and web serving. You're still responsible for your content, access control and configuration.
</details>

## Clean-up

When you've captured your evidence and shown it off, remove the lock and delete the resource groups:

```bash
az lock delete --name <your-lock-name> --resource-group rg-mzansi-prod
az group delete --name rg-mzansi-prod --yes --no-wait
az group delete --name rg-mzansi-learn --yes --no-wait
```

Also delete the test user in Entra ID and the policy assignments.

## Save your evidence: you've earned it

✅ A live website (screenshot it before clean-up)
✅ An architecture diagram
✅ A handover document that explains your decisions

**CV bullet:**
> Deployed a static website for a small business on Azure Storage, with cost budgets, region-restriction policy, resource locks and least-privilege (Reader) access for a contractor.

**LinkedIn post idea:** *"I just finished the KaraboNode Cloud Foundations series and put a Soweto clothing brand online on Azure, for cents a month. Here's what I learned about cost, access and governance…"* Add your diagram and repo link.

## What's next?

🎓 You've practised every area of **AZ-900**. Book the exam when you're ready.
➡️ Next on KaraboNode: the intermediate **Azure Networking Lab**.

---

**← Previous:** [Lab 05](../05-identity-governance/) · [Back to the series](../../)
