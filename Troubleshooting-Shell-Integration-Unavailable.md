**Note**: I've created a [minimal reproducible environment](https://github.com/saoudrizwan/shell-integration-problems) to help report issues with VSCode 1.93's new Terminal Shell Integration API. If the troubleshooting steps below don't resolve your issue, please follow the steps in that repository to report the problems you encounter.

***

- [What is Shell Integration?](#what-is-shell-integration)
- [How to Fix "Shell Integration Unavailable"](#how-to-fix-shell-integration-unavailable)
  - [Step 1: Update VSCode or Cursor](#step-1-update-vscode-or-cursor)
  - [Step 2: Configure VSCode to Use the Correct Shell](#step-2-configure-vscode-to-use-the-correct-shell)
  - [Step 3: Restart VSCode](#step-3-restart-vscode)
- [Additional Troubleshooting for Windows PowerShell Users](#additional-troubleshooting-for-windows-powershell-users)
  - [Understanding PowerShell Execution Policies](#understanding-powershell-execution-policies)
  - [Steps to Change the Execution Policy](#steps-to-change-the-execution-policy)
- [Still Having Trouble?](#still-having-trouble)
- [Unusual Terminal Output](#unusual-terminal-output)
- [Support](#support)

## What is Shell Integration?

Shell integration is a [new feature in VSCode 1.93](https://code.visualstudio.com/updates/v1_93#_terminal-shell-integration-api) that allows extensions like Cline to run commands in your terminal and read their output. Command output allows Cline to react to the result of the command on his own, without you having to handhold by copy-pasting the output in yourself. It's also quite powerful when running development servers as it allows Cline to fix errors as the server logs them.

## How to Fix "Shell Integration Unavailable"

### Step 1: Update VSCode or Cursor

First, make sure you're using the latest version of VSCode or Cursor:

1. Open VSCode or Cursor
2. Press `Cmd + Shift + P` (Mac) or `Ctrl + Shift + P` (Windows/Linux)
3. Type "Check for Updates" (VSCode) or "Attempt Update" (Cursor) and select it
4. Restart VSCode/Cursor after the update

### Step 2: Configure VSCode to Use the Correct Shell

1. Open VSCode
2. Press `Cmd + Shift + P` (Mac) or `Ctrl + Shift + P` (Windows/Linux)
3. Type "Terminal: Select Default Profile" and choose it
4. Select one of the supported shells: zsh, bash, fish, or PowerShell.

### Step 3: Restart VSCode

After making these changes:

1. Quit VSCode completely
2. Reopen VSCode
3. Start a new Cline session (you can resume your previous task and try to run the command again, this time with Cline being able to see the output)

## Additional Troubleshooting for Windows PowerShell Users

If you're using Windows and still experiencing issues with shell integration after trying the previous steps, make sure you're using an updated version of PowerShell (at least v7+).
  - Check your current PowerShell version by running: `$PSVersionTable.PSVersion`
  - If your version is below 7, [update PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/whats-new/migrating-from-windows-powershell-51-to-powershell-7?view=powershell-7.4#installing-powershell-7).

You may also need to adjust your PowerShell execution policy. By default, PowerShell restricts script execution for security reasons.

### Understanding PowerShell Execution Policies

PowerShell uses execution policies to determine which scripts can run on your system. Here are the most common policies:

- `Restricted`: No PowerShell scripts can run. This is the default setting.
- `AllSigned`: All scripts, including local ones, must be signed by a trusted publisher.
- `RemoteSigned`: Scripts created locally can run, but scripts downloaded from the internet must be signed.
- `Unrestricted`: No restrictions. Any script can run, though you will be warned before running internet-downloaded scripts.

For development work in VSCode, the `RemoteSigned` policy is generally recommended. It allows locally created scripts to run without restrictions while maintaining security for downloaded scripts. To learn more about PowerShell execution policies and understand the security implications of changing them, visit Microsoft's documentation: [About Execution Policies](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies).

### Steps to Change the Execution Policy

1. Open PowerShell as an Administrator: Press `Win + X` and select "Windows PowerShell (Administrator)" or "Windows Terminal (Administrator)".

2. Check Current Execution Policy by running this command:
     ```powershell
     Get-ExecutionPolicy
     ```
   - If the output is already `RemoteSigned`, `Unrestricted`, or `Bypass`, you likely don't need to change your execution policy. These policies should allow shell integration to work.
   - If the output is `Restricted` or `AllSigned`, you may need to change your policy to enable shell integration.

3. Change the Execution Policy by running the following command:
     ```powershell
     Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
     ```
   - This sets the policy to `RemoteSigned` for the current user only, which is safer than changing it system-wide.

4. Confirm the Change by typing `Y` and pressing Enter when prompted.

5. Verify the Policy Change by running `Get-ExecutionPolicy` again to confirm the new setting.

6. Restart VSCode and try the shell integration again.

## Still Having Trouble?

If you're still experiencing issues after trying the basic troubleshooting steps, you can try [manually installing shell integration](https://code.visualstudio.com/docs/terminal/shell-integration#_manual-installation). 

For example, if you use zsh:

1. Run `code ~/.zshrc` in the terminal to open your zsh configuration file.
2. Add the following line to your `~/.zshrc`:

```bash
[[ "$TERM_PROGRAM" == "vscode" ]] && . "$(code --locate-shell-integration-path zsh)"
```

3. Save the file.
4. Quit VSCode completely.
5. Reopen VSCode and start a new Cline session.

If you use PowerShell you would do:
1. Run `code $Profile` in the terminal to open your PowerShell profile.
2. Add the following line to the file:

```bash
if ($env:TERM_PROGRAM -eq "vscode") { . "$(code --locate-shell-integration-path pwsh)" }
```

## Unusual Terminal Output

If you're seeing unusual output with rectangles, lines, escape sequences, or control characters, it may be related to terminal customization tools. Common culprits include:

- Powerlevel10k: A zsh theme that adds visual elements to the prompt
- Oh My Zsh: A framework for managing zsh configurations
- Fish shell themes

To troubleshoot:

1. Temporarily disable these tools in your shell configuration file (e.g., `~/.zshrc` for Zsh)
2. If the issue resolves, gradually re-enable features to identify the conflicting component

For example, if you're using Powerlevel10k with Zsh, you can disable it by commenting out the relevant line in your `~/.zshrc` file:

```bash
# Comment out the Powerlevel10k source line
# source /path/to/powerlevel10k/powerlevel10k.zsh-theme
```

If disabling these customizations resolves the issue, you may need to find alternative configurations that are compatible with VSCode's shell integration feature.

## Support

If you've followed these steps and are still experiencing problems, please:

1. Check the [Cline GitHub Issues](https://github.com/clinebot/cline/issues) to see if others have reported similar problems
2. If not, create a new issue with details about your operating system, VSCode/Cursor version, and the steps you've tried

For additional help, join our [Discord](https://discord.gg/cline).