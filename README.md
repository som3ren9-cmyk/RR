# Cloud VM Runner via GitHub Actions & Tailscale

This repository provides automated GitHub Actions workflows to provision a temporary remote environment (Windows with Remote Desktop / Ubuntu with SSH) accessible securely via [Tailscale](https://tailscale.com/).

>  **Disclaimer:** This project is intended for educational, testing, and CI/CD debugging purposes only. Ensure your use complies with the [GitHub Acceptable Use Policies](https://docs.github.com/en/site-policy/acceptable-use-policies/github-acceptable-use-policies).

---

##  Prerequisites

Before running the workflow, make sure you have:

1. A **GitHub Account**.
2. A free **[Tailscale Account](https://login.tailscale.com/)**.
3. The **Tailscale client** installed and signed in on your personal device (PC, Mac, or phone).
4. A Remote Desktop client (e.g., **Microsoft Remote Desktop** / `mstsc`) for Windows sessions, or a terminal for SSH connections.

---

##  Setup Guide

### Step 1: Generate a Tailscale Auth Key

1. Log in to your [Tailscale Admin Console](https://login.tailscale.com/admin/settings/keys).
2. Go to **Settings** → **Keys**.
3. Under **Auth keys**, click **Generate auth key...**.
4. Configure the key settings:
   -  **Reusable:** Checked (allows you to trigger runs multiple times).
   - **Ephemeral:** Checked (automatically removes the machine from your Tailscale dashboard once the GitHub runner shuts down).
5. Click **Generate key** and copy the resulting string (it starts with `tskey-auth-...`).

---

### Step 2: Add the Secret to Your GitHub Repository

1. Navigate to your repository on GitHub.
2. Go to **Settings** → **Secrets and variables** → **Actions**.
3. Click the green button: **New repository secret**.
4. Fill in the fields:
   - **Name:** `TAILSCALE_AUTH_KEY`
   - **Secret:** Paste the auth key generated in Step 1.
5. Click **Add secret**.

---

### Step 3: Add the Workflow File

Create a YAML workflow file inside the `.github/workflows/` directory of your repository (e.g., `.github/workflows/runner.yml`).

Ensure your workflow includes:
- `on: workflow_dispatch` (to allow manual triggering).
- Steps to install Tailscale and authenticate using `${{ secrets.TAILSCALE_AUTH_KEY }}`.

Commit and push your changes to the `main` branch.

---

### Step 4: Run the Workflow

1. Go to the **Actions** tab at the top of your GitHub repository.
2. Select your workflow from the left sidebar.
3. Click the **Run workflow** dropdown button on the right, then click the green **Run workflow** button.
4. Click on the running job to view the real-time execution logs.

---

## How to Connect

Once the workflow finishes initializing, expand the connection step in the workflow logs:

### Option A: Windows (RDP)
1. Open **Remote Desktop Connection** (`mstsc` on Windows) on your local machine.
2. Make sure your local Tailscale application is connected.
3. Enter the **Tailscale IP address** shown in the workflow logs.
4. Log in using the credentials defined in the script:
   - **Default User:** `VPSAdmin`
   - **Default Password:** `WindowsVPS@2026#Pass`

### Option B: Linux / Ubuntu (SSH)
1. Open your local terminal / command prompt (with Tailscale active).
2. Connect via SSH:
   ```bash
   ssh runner@<TAILSCALE_IP>
