# My Dotfiles

Personal development environment configuration managed with [Dotbot](https://github.com/anishathalye/dotbot).

## ✨ Features

- 🚀 Automated setup with a single command
- 🔗 Efficient management via symbolic links
- 🔄 Version-controlled Dotbot via Git submodule
- 🍺 Homebrew package management with Brewfile
- 🖥️ Server environment optimized

## 📦 What's Included

- `.zshrc` - Zsh shell configuration
- `.gitconfig` - Git global settings
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
- Installs Homebrew packages from Brewfile
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

## 📝 License

MIT License

## 🤝 Contributing

If you have suggestions or improvements, feel free to open an issue or PR!

---

⚡ Powered by [Dotbot](https://github.com/anishathalye/dotbot) and [dotbot-brewfile](https://github.com/sobolevn/dotbot-brewfile)