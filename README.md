# Setting Up Claude Code in SAP Business Application Studio (BAS)

A step-by-step guide to installing and logging in to **Claude Code** inside **SAP Business Application Studio** on SAP BTP, including a fix for the common `localhost refused to connect` login error.

---

## Table of Contents

- [Prerequisites](#prerequisites)
- [Option 1: Install the Claude Code Extension](#option-1-install-the-claude-code-extension)
- [Option 2: Install the Claude Code CLI (Terminal)](#option-2-install-the-claude-code-cli-terminal)
- [Logging In (Recommended: via Terminal)](#logging-in-recommended-via-terminal)
- [Troubleshooting](#troubleshooting)
- [Usage Tips](#usage-tips)
- [References](#references)

---

## Prerequisites

- Access to **SAP Business Application Studio** (BTP trial or subscription) with a running dev space
- A **Claude account** on a paid plan (Pro, Max, Team, or Enterprise) **or** an **Anthropic Console** account with API credits

> **Note:** The free Claude plan does not include Claude Code.

> **Company policy:** Check with your IT / SAP team before sending company code to an external AI service.

---

## Option 1: Install the Claude Code Extension

BAS is built on Code-OSS (the open-source base of VS Code) and installs extensions from the Open VSX registry.

1. Open your dev space in BAS.
2. Open the Extensions view: `Ctrl + Shift + X`.
3. Search for **Claude Code** (publisher: Anthropic).
4. Click **Install**.
5. Open any file and click the **Spark icon** (✳) in the top-right editor toolbar to open the Claude Code panel.

The panel will ask how you want to log in:

| Option | When to use |
|---|---|
| **Claude.ai Subscription** | You have a Claude Pro, Max, Team, or Enterprise plan |
| **Anthropic Console** | You pay per use with API credits |
| **Bedrock, Foundry, or Vertex** | Enterprise cloud-provider setups (usually managed by IT) |

> ⚠️ **Known issue:** Logging in from the extension panel in BAS can fail with `localhost refused to connect`. Use the terminal login below instead. See [Troubleshooting](#localhost-refused-to-connect-err_connection_refused).

---

## Option 2: Install the Claude Code CLI (Terminal)

If the extension can't be installed (for example, extensions are restricted by your admin), use the CLI.

1. In BAS, open **Terminal → New Terminal**.
2. Install Claude Code (Node.js is pre-installed in BAS dev spaces):

   ```bash
   npm install -g @anthropic-ai/claude-code
   ```

3. Move into your project and start Claude:

   ```bash
   cd ~/projects/<your-project>
   claude
   ```

---

## Logging In (Recommended: via Terminal)

This method works reliably in BAS, and the extension panel usually picks up the same login afterwards.

1. Open **Terminal → New Terminal** in BAS.
2. Run:

   ```bash
   claude
   ```

3. Choose **Claude account with subscription** (or **Anthropic Console** if you use API credits).
4. Copy the login link shown in the terminal and open it in a new browser tab.
5. Sign in to your Claude account and click **Authorize**.
6. The page displays an **authorization code**. Copy it.
7. Paste the code into the terminal and press **Enter**.
8. You're logged in. If the extension panel still shows the login screen, reload the BAS browser tab.

### Alternative: API key

```bash
export ANTHROPIC_API_KEY=sk-ant-xxxxxxxx
claude
```

To keep it across sessions, add the `export` line to `~/.bashrc`.

> 🔒 Never commit your API key to Git.

---

## Troubleshooting

### `localhost refused to connect` (ERR_CONNECTION_REFUSED)

**Cause:** After you authorize, the browser redirects to `localhost` on *your laptop*, but Claude Code is running on SAP's *cloud* dev space, so nothing on your laptop is listening.

**Fix (recommended):** Use the [terminal login](#logging-in-recommended-via-terminal) instead of the extension panel.

**Workaround:** The code is in the error page's address bar:

```
http://localhost:<port>/callback?code=<THIS_PART>&state=...
```

Copy the value after `code=` and before `&state`, paste it into the extension's authorization code box, and click **Continue**. If it's rejected, try `CODE#STATE`, or repeat the steps faster, because codes expire quickly.

### Browser tab doesn't open after clicking "Claude.ai Subscription"

The panel shows **Continue in browser** with a URL. Copy the URL with the copy icon, open it in a new tab manually, or allow pop-ups for the BAS site.

### "Invalid code" error

The code has expired. Click **Back**, start the login again, and paste the new code promptly.

### `claude: command not found` after a dev space restart

Global npm packages can occasionally be lost. Reinstall:

```bash
npm install -g @anthropic-ai/claude-code
```

### Extension not found or won't install

Your BTP administrator may have restricted extensions. Ask them to allow it, or use the [CLI option](#option-2-install-the-claude-code-cli-terminal).

### Logged out after restart

Run `claude` in the terminal and repeat the login steps.

---

## Usage Tips

- **Open a project folder first.** Claude Code works on the folder you have open.
- Use either the **side panel** (Spark icon) or **`claude` in the terminal**.
- Example prompts:
  - `Explain this project structure`
  - `Create a simple CAP service with a Books entity`
  - `Add a Fiori Elements list report for this service`
  - `Find and fix errors in my CDS model`

---

## References

- [Claude Code in VS Code (official docs)](https://code.claude.com/docs/en/vs-code)
- [Claude Code on Open VSX](https://open-vsx.org/extension/Anthropic/claude-code)
- [SAP Business Application Studio (SAP Help Portal)](https://help.sap.com/docs/bas)
