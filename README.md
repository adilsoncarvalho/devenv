# DevEnv Ansible Playbook

This repository contains an [Ansible](https://www.ansible.com/) playbook for setting up and maintaining a consistent development environment across [macOS](https://www.apple.com/macos/) and [Ubuntu](https://ubuntu.com/) systems.

## Purpose

The playbook automates the installation and configuration of development tools and utilities, ensuring a consistent environment across different machines and operating systems.

## Current Features

- [Homebrew](https://brew.sh/):
  - Installs or updates Homebrew
  - Provides status information about the installed brew version

- [Git](https://git-scm.com/) installation and management:
  - Updates Apple Git via Xcode Command Line Tools on macOS
  - Installs Git via [Homebrew](https://brew.sh/) if not present
  - Provides status information about the installed Git version

- GitHub CLI installation:
  - Installs [GitHub CLI](https://cli.github.com/) (gh) via Homebrew on macOS
  - Installs GitHub CLI via apt on Ubuntu
  - Provides installation status and authentication instructions

- [Zsh](https://www.zsh.org/) configuration:
  - Installs Zsh if not present
  - Installs and configures [Oh My Zsh](https://ohmyz.sh/)
  - Sets up common plugins (git, gh, docker, kubectl, etc.)
  - Configures custom aliases for common commands
  - Sets Zsh as the default shell

- Font installation:
  - Installs [JetBrains Mono](https://www.jetbrains.com/lp/mono/) font on macOS
  - Provides a clean, modern monospace font optimized for coding

## Repository Structure

```
.
├── .github/
│   └── workflows/       # GitHub Actions workflow configurations
├── roles/
│   └── common/          # Common role for shared functionality
│       ├── defaults/    # Default variables for the role
│       ├── files/       # Static files used by the role
│       ├── handlers/    # Handlers for the role
│       ├── meta/        # Role metadata
│       ├── tasks/       # Tasks for the role
│       │   ├── brew.yml  # Homebrew installation and configuration tasks
│       │   ├── git.yml  # Git installation and configuration tasks
│       │   ├── gpg.yml  # GPG installation and configuration tasks
│       │   ├── gh.yml   # GitHub CLI installation and configuration tasks
│       │   ├── zsh.yml  # Zsh installation and configuration tasks
│       │   └── fonts.yml # Font installation tasks
│       ├── templates/   # Jinja2 templates
│       └── vars/        # Role variables
├── .editorconfig        # Editor configuration
├── .gitignore           # Git ignore rules
├── inventory.yml        # Ansible inventory file
└── playbook.yml         # Main Ansible playbook
```

## CI/CD

The repository uses GitHub Actions to test the playbook on both Ubuntu and macOS environments. Tests run on:
- Every push to `master`
- Every push to feature branches (`feat/**`)
- Pull requests targeting `master` or feature branches

## For AI Agents

This repository is AI-friendly and follows these conventions:

### Commit Messages
- Uses semantic commit messages (`feat:`, `fix:`, `ci:`, etc.)
- Each commit message has a clear title and description
- Descriptions use bullet points for multiple items
- Add `[skip CI]` to commit messages on commits that only changed the documentation

### Branch Strategy
- Main branch: `master`
- Feature branches: `feat/**`
- CI runs on all branches to ensure early feedback

### Development Workflow
1. Create a feature branch from master
2. Make changes and test locally
3. Push changes and create a PR
4. After the PR gets merged on GitHub, delete the local branch and update `master`

## Getting Started

To run the playbook locally:

```bash
ansible-playbook -i inventory.yml playbook.yml
```

You can also [use tags](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_tags.html) to run only parts of the playbook.

```bash
ansible-playbook -i inventory.yml playbook.yml --tags [TAG]
```

### Tag List

Here is a list with all the tags that we have in this playbook.

| Tag     | Description                                                                 |
|---------|-----------------------------------------------------------------------------|
| `brew`  | Installs and configures Homebrew                                            |
| `fonts` | Installs and configures JetBrains Mono font (macOS only)                    |
| `gh`    | Installs and configures GitHub CLI with authentication and settings         |
| `git`   | Installs and configures Git with proper version management                  |
| `gpg`   | Installs and configures GPG for secure commit signing                       |
| `zsh`   | Installs and configures Zsh with Oh My Zsh and custom settings              |

## Local Testing

You can set the following environment variables to avoid being prompted during playbook execution:

```bash
# GitHub Personal Access Token with required scopes
export GH_TOKEN='your_github_token'

# User information for GPG key generation
export USER_NAME='Your Name'
export USER_EMAIL='your.email@example.com'
```

## Requirements

- Python 3.x
- Ansible
- For macOS: Homebrew (will be installed if missing) 

## CI Requirements

The GitHub Actions workflow requires a Personal Access Token (PAT) with the following scopes:
- `repo` - For repository access
- `read:org` - For organization access
- `admin:public_key` - For managing SSH keys
- `admin:gpg_key` - For managing GPG keys

To set up the PAT:
1. Create a new [Personal Access Token (classic)](https://github.com/settings/tokens) in your GitHub account settings
2. Select the required scopes mentioned above
3. Add the token as a repository secret named `GH_TOKEN` in your repository settings 