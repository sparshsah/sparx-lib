Set up a new PC (personal computer),
which for me usually does NOT run the Microsoft Windows OS.

# macOS

## Human-Driven Steps

These tend to require hands and/or subjective taste,
but are generally pretty safe.

- Turn off fucking AutoCorrect:
  - System Settings > Keyboard > Input Sources > Edit > Correct Spelling Automatically.
  - System Settings > Keyboard > Input Sources > Text Replacements.
- Set dock location to LHS of screen.
  - You can also save/restore location/size/order as a plist.
- System Settings > Desktop & Dock > Hot Corners > Bottom Right > DESKTOP.
- System Settings > Desktop & Dock > Mission Control >
  Automatically rearrange Spaces based on most recent use > OFF.
- Quasi-`caffeinate` on charger:
  System Settings > Battery > Options >
  Prevent automatic sleeping on power adapter when the display is off > YES.
- Pin `~` to the LHS sidebar in Finder.
- Finder > Settings > Advanced > Show all filename extensions > ON.
- Install [Google Chrome](https://www.google.com/chrome/).
  - Set it as your default browser.
  - Make sure AutoFill and Password Manager is OFF.
  - Set file downloads destination to "Ask Every Time".
- Set up Terminal profile (text background color dark, opacity 90%, etc).
  - You can export/import this bundle as a `.terminal` file.
- [Install OpenAI Codex CLI by `curl`ing the standalone script](https://learn.chatgpt.com/docs/codex/cli#getting-started).
  - The standalone script is relatively dependency-free
    and sets Codex up to auto-update smoothly.

## Agent-Driven Steps

These can be driven by an AI agent consulting the human owner as appropriate.
Important: Make sure these steps are still consistent with latest up-to-date workflows/recipes!

- Seed [AGENTS.md](https://github.com/sparshsah/sparx-lib/blob/main/AGENTS.md).
- Install XCode Command Line Tools with `xcode-select --install`.
- Create a `.zshrc` with `touch ~/.zshrc`.
- Install [HomeBrew](https://brew.sh/) with
  `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`.
  - Also proceed with any appropriate immediate post-install instructions (environment setup, `brew doctor`, etc).
  - You can periodically update with `brew update && brew upgrade && brew cleanup`.
- [Generate a new SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent).
- Seed `~/.ssh/config`:
  ```
  Host github.com
      IgnoreUnknown UseKeychain
      AddKeysToAgent yes
      UseKeychain yes
      IdentityFile ~/.ssh/id_ed25519
  ```
  .
- Let Mac remember your SSH key:
  `/usr/bin/ssh-add --apple-use-keychain ~/.ssh/id_ed25519`.
- [Add your SSH key to GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).
- Add GitHub to known hosts:
  Run `ssh -T git@github.com`;
  On first connection,
  verify the displayed host-key fingerprint against
  [GitHub's published fingerprints](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints)
  before accepting it.
  - Avoid the blind `ssh-keyscan github.com >> ~/.ssh/known_hosts`,
    which is susceptible to MitM attacks.
- `mkdir` your directory structure (e.g. `~/Documents/repos/...`),
  and `git clone` whatever repos you want.
- [Install Anaconda](https://www.anaconda.com/docs/getting-started/anaconda/install/mac-cli-install).
  - Create a fresh Python virtual environment with `conda create --name=py_venv python anaconda`.
  - You can activate it with `conda activate py_venv`.
    Then, things like `jupyter lab` immediately work.
  - You can refresh/update/upgrade everything once in a while with
    `conda deactivate && conda remove --name=py_venv --all && conda update conda && conda create --name=py_venv python anaconda`.
  - TODO: Evaluate migrating from Anaconda
    (admittedly bloated/clunky but also convenient for local scientific workflows,
    esp if you can accept the nuke-everything-and-blind-upgrade routine)
    to [uv](https://docs.astral.sh/uv/) (with `pyproject.toml` +
    some HomeBrew-managed cross-dependencies like OpenMP/LibOMP for LightGBM)
    for lightweight, highly-configurable, reproducible, project-specific venvs.
- Prepend to your PATH by adding the following to your `~/.zshrc`:
  `export PATH="/path/to/your/code:/path/to/more/code:$PATH"`.
- [Install VS Code](https://code.visualstudio.com/download),
  optionally turning on Settings Sync and opening your notes doc.
