---
Order: 16
Area: editor
TOCTitle: Workspace Trust
ContentId: 51280c26-f78b-4f9c-997f-8350bd6ed07f
PageTitle: Visual Studio Code Workspace Trust security
DateApproved: 5/5/2021
MetaDescription: Visual Studio Code workspace trust folder security
---
# Workspace Trust

Visual Studio Code takes security seriously and wants to help you safely browse and edit code no matter the source or original authors. The Workspace Trust feature lets you decide whether your project folders should allow or restrict automatic code execution.

![Trust this folder dialog](images/workspace-trust/workspace-trust-dialog.png)

>**Note**: When in doubt, leave a folder in [Restricted Mode](#restricted-mode). You can always [enable trust](#trusting-a-workspace) later.

## Safe code browsing

It's great that there is so much source code available on public repositories and file shares. No matter the coding task or problem, there is probably already a good solution available somewhere. It is also great that there are so many powerful coding tools available to help you understand, debug, and optimizes your code. However, using open source code and tools does have risks and you can leave yourself open to malicious code execution and exploits.

Workspace Trust provides an extra layer of security when working with unfamiliar code by preventing automatic code execution when a workspace is open in "Restricted Mode".

> **Note**: The terms "workspace" and "folder" are used widely in the VS Code UI and documentation. You can think of a "workspace" as a folder with extra metadata created and used by VS Code.

## Restricted Mode

When prompted by the Workspace Trust dialog, if you choose **No, I don't trust the authors**, VS Code will go into Restricted Mode to prevent code execution. The workbench will display a banner at the top with links to **Manage** your folder via the Workspace Trust editor and **Learn More** taking you to documentation.

![Workspace Trust Restricted Mode banner](images/workspace-trust/restricted-mode-banner.png)

You will also see a Restricted Mode badge in the Status bar.

![Workspace Trust Restricted Mode Status bar badge](images/workspace-trust/restricted-mode-status-bar.png)

Restricted Mode tries to prevent automatic code execution by disabling or limiting the operation of several VS Code features: tasks, debugging, workspace settings, and extensions.

To see the full list of features disabled in Restricted Mode, you can open the Workspace Trust editor via the **Manage** link in the banner or by clicking the Restricted Mode badge in the Status bar.

![Workspace Trust editor](images/workspace-trust/workspace-trust-editor.png)

### Tasks

Tasks can run scripts and tool binaries and because tasks definitions are defined in the workspace `.vscode` folder, they are part of the committed source code for a repo and shared to every user of that repo. Were someone to create a malicious task, it could be unknownly run by anyone who cloned that repository.

If you try to run or even enumerate tasks (**Terminal** > **Run Task...**) while in Restricted Mode, VS Code will display a prompt to trust the folder and continue executing the task. Cancelling the dialog, leaves VS Code in Restricted Mode.

![Workspace Trust Restricted Mode tasks dialog](images/workspace-trust/restricted-mode-tasks-dialog.png)

### Debugging

Similar to running a VS Code task, debug extensions can run debugger binaries when launching a debug session. For that reason, debugging is also disabled when a folder is open in Restricted Mode.

If you try to start a debug session (**Run** > **Start Debugging**) while in Restricted Mode, VS Code will display a prompt to trust the folder and continue launching the debugger. Cancelling the dialog, leave VS Code in Restricted Mode and does not start the debug session.

![Workspace Trust Restricted Mode debugging dialog](images/workspace-trust/restricted-mode-debugging-dialog.png)

### Workspace settings

Workspace settings are stored in the `.vscode` folder at the root of your workspace and are therefore shared by anyone who clones the workspace repository. Some settings contain paths to executables (for example, linter binaries), which if set to point to malicious code, could do damage. For this reason, there are a set of workspace settings that are disabled when running in Restricted Mode.

![Workspace Trust editor workspace settings link](images/workspace-trust-workspace-settings-link.png)

In the Workspace Trust editor, there is a link to display the workspace settings that aren't being applied by bringing up the Settings editor scoped by the `@tag:requireTrustedWorkspace` tag.

![Settings editor scoped by the requireTrustedWorkspace tag](images/workspace-trust/requireTrustedWorkspace-settings.png)

### Extensions

The VS Code extensions ecosystem is incredibly rich and diverse. People have created extensions to help with just about any programming task or editor customization. Some extensions provide full programming language support (IntelliSense, debugging, code analysis) and others let you play music or
have virtual [pets](https://marketplace.visualstudio.com/items?itemName=tonybaloney.vscode-pets).

Most extensions run code on your behalf and so could potentially do harm. And some extensions have settings that could cause them to act maliciously if configured to run an unexpected executable. For this reason, extensions, if they have not explicitly opted into Workspace Trust, are disabled by default in Restricted Mode.

![Workspace Trust disabled extensions link](images/workspace-trust/disabled-extensions-link.png)

You can review installed extension status by clicking the **extensions are disabled or have limited functionality** link in the Workspace Trust editor and this will disply the Extensions view scoped with the `@workspaceUnsupported` filter.



**Disabled in Restricted Mode** section

Extension authors has not updated their extensions for Workspace Trust or set a not supporting Workspace Trust

**Limited in Restricted Mode** section

Badge showing "This extension has limited features because the current workspace is not trusted".

Installing extensions will cause a prompt. You can install an extension without enabling Workspace Trust but the extension will be disabled.

## Trusting a workspace

Initial dialog "Do you trust the authors of the files in this folder?"

Yes, I trust the authors

No, I don't trust the authors

Trust the parent option

### Bring up the Workspace Trust editor

When trusted:

Gear - **Manage Workspace Trust**
Command Palette - **Workspaces: Manage Workspace Trust**

When in Restricted Mode:

Restricted Mode banner **Manage** link.
Restricted Mode Status bar item

### Trusting an empty window

A trusted window (only lasts for the session) unless use the `security.workspace.trust.emptyWindow` setting.

## Selecting folders

Folders added to the list as you trust a folder. You can also manually add or modify the trusted folder list.

**Add Folder** button brings up platform Folder dialog with **Trust Folder** button.
Can not edit folder path directly? Could in earlier demo video, now goes to platform folder dialog

example for selecting parent folders

See a different message in Restricted Mode square and not **Don't Trust** button.

### Folder configurations

You can trust a parent folder and all subfolders will be trusted. This allows you to control Workspace Trust via a repository's location on disk.

For example you could put all trusted repos unter a "TrustedRepos" parent folder and unfamiliar repos under another parent folder. You would trust the "TrustedRepos" folder and never trust any folders unter "UntrustedRepos".

* TrustedRepos - Clone trusted repositories under this parent folder
* UntrustedRepos - Clone experimental or unfamiliar repositories under this parent folder

You also group and set trust on your repositories by grouping them under organization-base parent folders.

* github/microsoft - Clone an organizations repositories under this parent folder
* github/{myforks} - place your forks under this parent folder
* local

## Enabling extensions

Talk about `extensions.supportUntrustedWorkspaces`

## Installing new extensions (need this??)

## Settings

* `security.workspace.trust.enabled` - Enable Workspace Trust feature. Default is true.
* `security.workspace.trust.startupPrompt` - Whether to show the Workspace Trust dialog on startup. Default is to only show once per distinct workspace.
* `security.workspace.trust.emptyWindow` - Whether to always trust an empty window (no open folder). Default is false.
* `security.workspace.trust.untrustedFiles` - TBD
* `extensions.supportUntrustedWorkspaces` - Override extension Workspace Trust declarations. Either true or false. TBD requires a reload?

## Special configurations

### Remote extensions

SSH - paths are relative to the remote machine

WSL - paths are relative to WSL instance (/mnt/) (might map to already trusted local path)

Containers

### Codespaces (move to docs/remote/codespaces?)

Paths a little weird

## Next steps

Read on to find out about:

* [Workspace Trust for extension authors](/api/extension-guides/workspace-trust.md) - TBD

## Common questions

### Can I still edit my source code in Restricted Mode?

Yes, you can still browse and edit source code in Restricted Mode and VS Code's built-in Git integration works the same.

### Where did my installed extensions go?

In Restricted Mode, any extension that doesn't support Workspace Trust will be disabled and all UI elements such as Activity bar icons and commands will not be displayed.

Mention `extensions.supportUntrustedWorkspaces` setting but emphasize to use with care.

List of popular extensions that currently need this override.

### Can I disable the Workspace Trust feature?

You can but it is not recommended. If you don't want VS Code to check for Workspace Trust when opening a new folder or repository, you can set `security.workspace.trust.enabled` to false. VS Code will then behave as it did before release version 1.57.

### How do I untrust a folder/workspace?

Bring up Workspace Trust editor (**Workspaces: Manage Workspace Trust** from the Command Palette) and select **Don't Trust** button. You can also remove the folder from the **Trusted Folders & Workspaces** list.

### Why don't I see the "Don't Trust" button?

Mention trust via parent folders