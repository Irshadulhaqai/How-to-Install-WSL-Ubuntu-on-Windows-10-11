# 🚀 Claude Code Router: Master Setup Guide (WSL + Gemini)

[![Watch the Tutorial](https://img.youtube.com/vi/Kerha8oWWug/maxresdefault.jpg)](https://youtu.be/Kerha8oWWug)

> **📺 Watch the Video:** [Click here to watch the full step-by-step tutorial](https://youtu.be/Kerha8oWWug)  

This repository is the official guide for my YouTube tutorial. It helps you set up Claude Code on Windows using WSL2 and powers it with Google Gemini using the Claude Code Router.

---

# 🛠 Step 0: Install Windows Subsystem for Linux (WSL) 🐧

**Important:** 
This is the first thing you need to do. We are setting up a Linux environment where **Claude Code** will run.

**Why use WSL?**
Using WSL (Ubuntu) is highly recommended over PowerShell because it reduces the chance of errors and uses fewer tokens during coding


## 1. Open PowerShell as Administrator

* Press the **Windows** key
* Search for **PowerShell**
* Click **Run as administrator**
* Click **Yes**

## 2. Run the Install Command

```bash
wsl --install
```

## 3. Downloading & Launching

* Windows will download and install WSL
* Ubuntu will launch automatically
* Stay in the same window while it provisions

## 4. Set Your Username & Password

* Enter a **username**
* Enter a **password**
* Confirm the password

> ⚠️ Password typing is hidden. This is normal in Linux.

## 5. Success

When you see something like:

```bash
hammad@PC:/mnt/c/WINDOWS/system32$

```

Keep this terminal open and continue.

---

# ⚡ Method 1 — Automatic Speedrun Setup (Recommended)

## Run installer script
Run this inside **WSL / Ubuntu terminal**.

```bash
curl -sSL https://raw.githubusercontent.com/devhammad0/claude-code-router-setup/main/scripts/install.sh | bash

```

> After the script finishes:

```bash
source ~/.bashrc
```

## 2. Set Your Google API Key

Get your key from:  
https://aistudio.google.com/  

Then run:  
Replace `YOUR_KEY_HERE` with your key 

```bash
echo 'export GOOGLE_API_KEY="YOUR_KEY_HERE"' >> ~/.bashrc
```

## 3. Reload your Environment

```bash
source ~/.bashrc
```

## 4. Laucnh Claude Code Router

```bash
ccr code
```

---

# 🛠 **Method 2: Manual Installation**

Use this if you want to install everything step-by-step.

---

## 1. **Update Linux Packages**

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl git build-essential
```

---

## 2. **Install Node.js (via NVM)**

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
source ~/.bashrc
nvm install --lts
```

## 3. **Verify Installation**:
   ```
   node -v
   npm -v
   ```

---

## ⚙️ Configuration Setup

## Step 1: Check Your Shell (Important)

Run:

```bash
echo $SHELL
```

* If output contains `/bash` → use `~/.bashrc`
* If output contains `/zsh` → use `~/.zshrc`

---

## 2. Install Claude Code & CCR Router

```bash
npm install -g @anthropic-ai/claude-code @musistudio/claude-code-router
```

## Step 3: Create Config File (Google Gemini)

```bash
mkdir -p ~/.claude-code-router ~/.claude
```

Run the following command to create your configuration:

```json
cat > ~/.claude-code-router/config.json << 'EOF'
{
  "LOG": true,
  "LOG_LEVEL": "info",
  "HOST": "127.0.0.1",
  "PORT": 3456,
  "APIKEY": "your_secure_password_here",
  "API_TIMEOUT_MS": 600000,
  "Providers": [
    {
      "name": "gemini",
      "api_base_url": "https://generativelanguage.googleapis.com/v1beta/models/",
      "api_key": "$GOOGLE_API_KEY",
      "models": [
        "gemini-3-flash-preview",
        "gemini-3-pro-preview",
        "gemini-2.5-flash-lite",
        "gemini-2.0-flash"
      ],
      "transformer": { "use": ["gemini"] }
    }
  ],
  "Router": {
    "default": "gemini,gemini-2.5-flash-lite",
    "background": "gemini,gemini-2.5-flash-lite",
    "think": "gemini,gemini-2.5-flash-lite",
    "longContext": "gemini,gemini-2.5-flash-lite",
    "longContextThreshold": 60000
  }
}
EOF
```

---



## 📊 Model Performance & Quota (Free Tier)

Before you run the agent, check the current limits for the models included in the config above. These limits are reset at **Midnight Pacific Time (PT)**.

| Model Name | RPM (Req/Min) | TPM (Tokens/Min) | RPD (Req/Day) | Status/Notes |
| :--- | :---: | :---: | :---: | :--- |
| **gemini-2.5-flash-lite** | **30** | 1,000,000 | **1,500** | ✅ **Recommended for Coding** |
| **gemini-2.0-flash** | 10 | 1,000,000 | 1,000 | Stable legacy option |
| **gemini-3-flash-preview**| 2 | 100,000 | 100 | ⚠️ Strict preview limits |
| **gemini-3-pro-preview** | 0 - 2* | 32,000 | 5 - 10 | 🚫 Effectively Paid-Only |

---

> **Free Tier Quota Note:**  
>
> * **RPM** (Requests Per Minute) is strictly enforced. If Claude Code fires multiple background file reads, you may hit a brief 429 error.
> * **Gemini 3 Pro Preview** is currently highly restricted for free users; frequent use in the `think` or `longContext` routes will likely exhaust your daily quota (RPD) in minutes.
>
> **TIP:**  
> ***Why use Flash-Lite?** *Claude Code performs many small background tasks (reading files, checking status). Using a model with less than 10 RPM will cause "Rate Limit" errors almost immediately. Flash-Lite is the only free model that provides a smooth experience for complex coding.*


---

## 6. **Verify Config File**:
   ```
   cat ~/.claude-code-router/config.json
   ```
   - Check if it matches the JSON above.

## 7. **Add Google API Key to Ubuntu Environment** 🌐:
   - Check your shell:
     ```
     echo $SHELL
     ```
     - Most likely: `/bin/bash`
   - Add the key to `.bashrc`:
     ```
     echo 'export GOOGLE_API_KEY="YOUR_KEY_HERE"' >> ~/.bashrc
     source ~/.bashrc
     ```
     - Replace `YOUR_KEY_HERE` with your actual API key.

## 8. **Verify Key**:
   ```
   echo $GOOGLE_API_KEY
   ```
   - Should display your key.

## 9. **Verify Installations**:
   ```
   claude --version
   ccr
   ```
   - `ccr` should show command options if installed correctly.

**Setup Done!** 🎉 You're ready to use Claude.

## Step 5: Daily Workflow (Ubuntu Version) 📅
Use two terminals for smooth operation.

## 1. **Terminal 1: Start Router** ▶️:
   ```
   ccr start
   ```
   - Wait for: **Service started successfully**.

## 2. **Terminal 2: Use Claude** 🧑‍💻:
   - Create a project folder:
     ```
     mkdir -p ~/your-project
     cd ~/your-project
     ```
   - Activate and use Claude:
     ```
     eval "$(ccr activate)"
     claude
     ```
     - OR directly: `ccr code`


## Step 3: Set API Key

Use the shell configuration file you identified in Step 1. Replace `YOUR_KEY_HERE` with your actual Google AI Studio API key.

**If using bash:**

```bash
echo 'export GOOGLE_API_KEY="YOUR_KEY_HERE"' >> ~/.bashrc
source ~/.bashrc
```

**If using zsh:**

```bash
echo 'export GOOGLE_API_KEY="YOUR_KEY_HERE"' >> ~/.zshrc
source ~/.zshrc
```

---

## 🚀 How to Run

```bash
ccr code
```

---

## 💡 Essential Commands

* `ccr start` / `ccr stop` / `ccr status` — Manage the background router service.
* `ccr model` — Quick interactive configuration of your model providers.
* `ccr ui` → Dashboard at `http://localhost:3456`
---


## ❓ Troubleshooting

### 1. Service Startup Timeout  

If `ccr code` shows a "Timeout" error, the background service might just need a manual kickstart. Run these separately:  

```bash
ccr start
ccr code
```

## 2. Status shows "Not Running"  

This usually means a "ghost" process is already using the port. Run this to clear it:

```bash
# Find any process on port 3456
sudo lsof -i :3456
# Use the PID found to kill it: sudo kill -9 <PID>
```

## 3. Verification Commands  

Use these commands to verify your setup:

- `ccr status` — Check if the background service is running.
- `/status` — (Inside Claude) Verify the Base URL is 127.0.0.1:3456.

--- 

## Step 6: Test Claude ✅
In the Claude session, type:
```
hi
```
- Expected reply: **Hello! Claude is ready.**

## Troubleshooting ⚠️
- **API Key Issues**: Ensure `$GOOGLE_API_KEY` is set correctly.
- **Port Conflicts**: Change `"PORT"` in `config.json` if 3456 is in use.
- **Rate Limits**: Gemini is free but has daily limits—monitor usage.
- **Errors**: Check logs (LOG is enabled in config).

## Additional Notes 📝
- This setup routes to Gemini models like `gemini-2.5-flash` for Claude-like coding assistance.
- Inspired by YouTube tutorials—credit to original creators!
- Update tools periodically: `sudo npm update -g @anthropic-ai/claude-code @musistudio/claude-code-router`

Enjoy your free Claude AI setup! If you have issues, feel free to ask. 😊

**Maintained by Irshad Ul Haq** | [LinkedIn Profile](https://www.linkedin.com/in/irshad-ulhaq-ai/)
