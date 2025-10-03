# My Dotfiles

Personal development environment configuration managed with [Dotbot](https://github.com/anishathalye/dotbot).

## ✨ Features

- 🚀 Automated setup with a single command
- 🔗 Efficient management via symbolic links
- 🔄 Version-controlled Dotbot via Git submodule
- 🍺 Homebrew package management with Brewfile
- 🐳 Dev Container integration support
- 💻 Mac and Linux support

## 📦 What's Included

- `.zshrc` - Zsh shell configuration
- `.gitconfig` - Git global settings
- `.settings` - VS Code settings
- `Brewfile` - Homebrew package list

## 🚀 Quick Start

### Install on a New System

```bash
git clone https://github.com/blAs1N/dotfiles.git ~/.dotfiles
cd ~/.dotfiles
./install
```

That's it! All your configurations will be automatically applied with just 3 commands.

## 📋 Detailed Installation Guide

### 1. Clone the Repository

```bash
git clone https://github.com/blAs1N/dotfiles.git ~/.dotfiles
cd ~/.dotfiles
```

### 2. Initialize Dotbot Submodules

```bash
git submodule update --init --recursive
```

### 3. Run the Install Script

```bash
./install
```

The install script automatically performs the following:
- Backs up existing configuration files
- Creates symbolic links
- Creates necessary directories
- Installs Homebrew packages from Brewfile (Mac only)
- Executes additional shell commands

## 🔧 Usage

### Modifying Configuration Files

1. Edit files in the `~/.dotfiles` directory
2. Changes are immediately reflected in your actual config (via symlinks)
3. Commit and push with Git

```bash
cd ~/.dotfiles
# After editing files
git add .
git commit -m "Update zsh config"
git push
```

### Adding New Configuration Files

1. Add files to `~/.dotfiles`
2. Add link configuration to `install.conf.yaml`
3. Re-run `./install`

**install.conf.yaml example:**

```yaml
- link:
    ~/.zshrc: zshrc
    ~/.gitconfig: gitconfig
    ~/.vimrc: vimrc
```

### Managing Homebrew Packages

#### Dump Current Packages

```bash
cd ~/.dotfiles
brew bundle dump --file=~/.dotfiles/Brewfile --force
git add Brewfile
git commit -m "Update Brewfile"
git push
```

#### Install Packages from Brewfile

```bash
cd ~/.dotfiles
brew bundle
# Or just run ./install again
```

The Brewfile includes:
- **tap** - Homebrew taps
- **brew** - Command-line tools
- **cask** - GUI applications
- **mas** - Mac App Store apps
- **vscode** - VS code extensions

### Updating Dotbot

```bash
git submodule update --remote dotbot
git submodule update --remote dotbot-brewfile
git add dotbot dotbot-brewfile
git commit -m "Update Dotbot plugins"
```

## 📂 Directory Structure

```
~/.dotfiles/
├── install                  # Installation script
├── install.conf.yaml        # Dotbot configuration
├── dotbot/                  # Dotbot submodule
├── dotbot-brewfile/         # Brewfile plugin submodule
├── Brewfile                 # Homebrew packages
├── config/zshrc             # Zsh configuration
├── config/gitconfig         # Git configuration
├── config/karabiner.json    # Karabiner configuration
├── config/neofetch.json     # Neofetch configuration
├── ray.rayconfig            # Raycast configuration (You cannot use it!)
└── README.md                # This file
```

## ⚙️ install.conf.yaml Configuration

The `install.conf.yaml` file defines how Dotbot operates:

```yaml
- defaults:
    link:
      relink: true        # Overwrite existing links
      create: true        # Auto-create necessary directories
      force: false        # Force overwrite existing files (use with caution!)

- clean: ['~']            # Clean up broken symlinks

- link:
    ~/.zshrc: zshrc
    ~/.gitconfig: gitconfig
    ~/.vimrc: vimrc

- brewfile:
    file: Brewfile
    stdout: true
    stderr: true

- shell:
  - [git submodule update --init --recursive, Installing/updating submodules]
  - [echo "Installation complete!", Finishing up]
```

## 🐳 Dev Container Integration

### Option 1: Personal postCreateCommand (Recommended)

Keep your dotfiles setup personal without modifying `.devcontainer/devcontainer.json`.

Create `.devcontainer/docker-compose.override.yml`:

```yaml
version: '3.8'
services:
  app:
    # Your personal post-create commands
    # This file can be gitignored
```

Or use VS Code's personal settings. Add to your **User Settings** (not workspace):

```json
{
  "dev.containers.defaultExtensions": [],
  "terminal.integrated.shellArgs.linux": ["-c", "test -d ~/.dotfiles || (git clone https://github.com/blAs1N/dotfiles.git ~/.dotfiles && cd ~/.dotfiles && ./install)"]
}
```

### Option 2: Manual Setup in Container

After the container starts:

```bash
# Inside the dev container
git clone https://github.com/blAs1N/dotfiles.git ~/.dotfiles
cd ~/.dotfiles
./install
```

### Option 3: Dockerfile Layer (Team-Optional)

If you want to make it available but optional for your team, add to `Dockerfile`:

```dockerfile
# Optional: Add dotfiles support
# Team members can enable this by uncommenting
# RUN git clone https://github.com/blAs1N/dotfiles.git /home/vscode/.dotfiles \
#     && cd /home/vscode/.dotfiles \
#     && ./install
```

### Option 4: Personal dotfiles Configuration

VS Code has built-in dotfiles support! Add to your **User Settings**:

```json
{
  "dotfiles.repository": "blAs1N/dotfiles",
  "dotfiles.targetPath": "~/.dotfiles",
  "dotfiles.installCommand": "~/.dotfiles/install"
}
```

This automatically clones and installs your dotfiles in any dev container!

### Skip Brewfile in Containers

If you want to skip Homebrew installation in dev containers, modify `install.conf.yaml`:

```yaml
- brewfile:
    file: Brewfile
    stdout: true
    stderr: true
    # Only run on macOS
    if: '[ "$(uname)" = "Darwin" ]'
```

Or create a separate `install-container.conf.yaml` without the brewfile section.

## ⚠️ Raycast Configuration Note

Raycast currently does not support automatic import via CLI.

**Manual import method:**
1. Open Raycast (⌘ + Space)
2. Search for "Import"
3. Select `~/.dotfiles/ray.rayconfig`

## 🛠️ Troubleshooting

### Symlinks Not Created

```bash
# Check links manually
ls -la ~ | grep "^l"

# Reinstall
./install
```

### Conflicts with Existing Config Files

```bash
# Backup existing files
mv ~/.zshrc ~/.zshrc.backup
mv ~/.gitconfig ~/.gitconfig.backup

# Reinstall
./install
```

### Dotbot Submodule Issues

```bash
git submodule update --init --recursive
```

### Brewfile Installation Fails

```bash
# Make sure Homebrew is installed
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Try installing packages manually
cd ~/.dotfiles
brew bundle
```

### Dev Container Dotfiles Not Loading

Check that:
1. Git is available in the container
2. You have network access to clone the repository
3. The install script has execute permissions (`chmod +x install`)

## 🔐 Security

Do not include sensitive information (tokens, passwords, etc.) in your dotfiles!

Add to `.gitignore`:
```
*secret*
*token*
.env
.devcontainer/docker-compose.override.yml
```

Use environment variables or separate files for sensitive data.

## 📚 Additional Resources

- [Dotbot Official Documentation](https://github.com/anishathalye/dotbot)
- [Dotbot Configuration Examples](https://github.com/anishathalye/dotbot/wiki/Configuration)
- [dotbot-brewfile Plugin](https://github.com/sobolevn/dotbot-brewfile)
- [Homebrew Bundle](https://github.com/Homebrew/homebrew-bundle)
- [VS Code Dev Containers Dotfiles](https://code.visualstudio.com/docs/devcontainers/containers#_personalizing-with-dotfile-repositories)

## 📝 License

MIT License

## 🤝 Contributing

If you have suggestions or improvements, feel free to open an issue or PR!

---

⚡ Powered by [Dotbot](https://github.com/anishathalye/dotbot) and [dotbot-brewfile](https://github.com/sobolevn/dotbot-brewfile)