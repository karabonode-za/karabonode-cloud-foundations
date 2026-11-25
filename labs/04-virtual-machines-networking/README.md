# Lab 04 · Your first virtual machine

> ☁️ Cloud Foundations · **Lab 4 of 6** · ⏱ ~1.5 hours · 💰 Cents (**delete it when done!**) · 🎯 AZ-900: Compute & networking

---

## The story

A friend tells Lerato: *"Just rent a server for the website."* Before deciding, you want to try it: run a small **web server** in Azure and see what it takes to keep one running.

## What you'll learn

- What a **virtual machine (VM)** is, and where it fits: **IaaS vs PaaS vs SaaS**
- How a **virtual network (VNet)**, **public IP** and **network security group (NSG)** let people reach a server, or keep them out
- Why a running VM costs money, and how to **stop** it properly

## Before you start

- [ ] Labs 01–03 done
- [ ] Your budget alert from Lab 02 is set

## Steps

### 1. Create a small Linux VM

Open **Cloud Shell (Bash)** and run:

```bash
az vm create --resource-group rg-mzansi-learn --name vm-mzansi-web \
  --image Ubuntu2204 --size Standard_B1s \
  --admin-username azureuser --generate-ssh-keys
```

It takes a minute or two. Note the **publicIpAddress** in the output.

> 💡 If `Standard_B1s` isn't available, try `Standard_B2ats_v2`.

✅ **Done when:** the VM shows **Running** in the portal.

### 2. See what Azure built for you

Open `rg-mzansi-learn`. One command created **several** resources:

| Resource | What it does |
|---|---|
| Virtual machine | The computer itself |
| Disk | Its hard drive |
| Network interface (NIC) | Its network card |
| Virtual network (VNet) | A private network in Azure |
| Public IP address | Its address on the internet |
| Network security group (NSG) | A firewall: decides what traffic is allowed in |

✅ **Done when:** you can say what each one does.

### 3. Install a web server

```bash
az vm run-command invoke --resource-group rg-mzansi-learn --name vm-mzansi-web \
  --command-id RunShellScript --scripts "sudo apt-get update && sudo apt-get install -y nginx"
```

Now open `http://<your-public-ip>` in your browser.

❌ It doesn't load. **Why?** The NSG only allows SSH (port 22). Web traffic uses port 80, and it's blocked.

### 4. Open the firewall for the website

```bash
az vm open-port --resource-group rg-mzansi-learn --name vm-mzansi-web --port 80
```

Refresh the browser: ✅ **Welcome to nginx!** You're serving a web page from the cloud.

Open the NSG in the portal → **Inbound security rules** and find the new rule for port 80.

✅ **Done when:** you see the nginx page and the port 80 rule.

### 5. IaaS, PaaS or SaaS?

With a VM, **you** must update the operating system, patch security holes and keep nginx running. That's **IaaS** (Infrastructure as a Service): Azure gives you the computer, you look after everything on it.

✍️ In your notes, fill in this table for Mzansi Threads:

| Option | Example | Who patches the OS? | Good for Mzansi Threads? Why? |
|---|---|---|---|
| IaaS | This VM | | |
| PaaS | Azure Storage static website / App Service | | |
| SaaS | Shopify, Instagram Shop | | |

## Check yourself

<details><summary>1. Why couldn't you see the website at first?</summary>
The NSG only allowed port 22 (SSH). Port 80 (HTTP) had to be opened.
</details>

<details><summary>2. Does a stopped VM cost money?</summary>
If it's <strong>stopped (deallocated)</strong> from the portal or CLI, you don't pay for compute, but you still pay a little for its disk. If you only shut it down from inside the OS, Azure may keep charging. Deleting it stops all costs.
</details>

<details><summary>3. In the shared responsibility model, who is responsible for patching this VM's OS?</summary>
You, because it's IaaS. With PaaS and SaaS, the provider takes on more of that work.
</details>

## Clean-up (required!)

A VM costs money every hour it runs. Delete it and everything it created, but keep the storage account:

```bash
az vm delete -g rg-mzansi-learn -n vm-mzansi-web --yes
az resource list -g rg-mzansi-learn -o table
```

Then delete the leftover **disk, NIC, public IP, NSG and VNet** in the portal (tick them in the resource group → **Delete**). Keep only your **storage account**.

✅ **Done when:** `rg-mzansi-learn` contains only your storage account.

## Save your evidence

`my-cloud-foundations/04-vm/notes.md`: a screenshot of the nginx page (hide the IP), your IaaS/PaaS/SaaS table, and your recommendation for Lerato.

---

**← Previous:** [Lab 03](../03-storage/) · **Next →** [Lab 05 · Who can do what](../05-identity-governance/)
