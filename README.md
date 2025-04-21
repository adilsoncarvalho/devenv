# DevEnv Ansible Playbook

This repository contains an [Ansible](https://www.ansible.com/) playbook for setting up and maintaining a consistent development environment across [macOS](https://www.apple.com/macos/) and [Ubuntu](https://ubuntu.com/) systems.

## Purpose

The playbook automates the installation and configuration of development tools and utilities, ensuring a consistent environment across different machines and operating systems.

## Current Features

- [Git](https://git-scm.com/) installation and management:
  - Updates Apple Git via Xcode Command Line Tools on macOS
  - Installs Git via [Homebrew](https://brew.sh/) if not present
  - Configures Git user and GPG settings
  - Provides status information about the installed Git version

- GitHub CLI installation:
  - Installs [GitHub CLI](https://cli.github.com/) (gh) via Homebrew on macOS
  - Installs GitHub CLI via apt on Ubuntu
  - Configures GitHub CLI with GPG integration and custom aliases
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

- GPG configuration:
  - Installs and configures GPG for secure commit signing
  - Sets up GPG integration with Git and GitHub CLI
  - Provides instructions for generating and managing GPG keys

- Homebrew package management:
  - Installs [Homebrew](https://brew.sh/) on macOS
  - Manages Homebrew packages and dependencies
  - Provides status information about installed packages

- Oh My Zsh customization:
  - Installs and configures [Oh My Zsh](https://ohmyz.sh/)
  - Sets up themes, plugins, and aliases for an enhanced shell experience
  - Provides a streamlined and customizable Zsh environment

## Repository Structure

```
.
├── ansible.cfg          # Ansible configuration file
├── inventory.yml        # Ansible inventory file
├── playbook.yml         # Main Ansible playbook
├── README.md            # Project documentation
├── roles/               # Ansible roles directory
│   ├── git/             # Git-related tasks
│   │   ├── tasks/       # Task files for Git configuration
│   │   │   ├── config-gpg.yml # GPG configuration tasks
│   │   │   ├── config-user.yml # Git user configuration tasks
│   │   │   ├── macos.yml    # macOS-specific Git tasks
│   │   │   ├── main.yml     # Main Git tasks
│   │   │   └── ubuntu.yml   # Ubuntu-specific Git tasks
│   ├── github-cli/      # GitHub CLI-related tasks
│   │   ├── tasks/       # Task files for GitHub CLI configuration
│   │   │   ├── config-gpg.yml # GPG integration tasks
│   │   │   ├── config.yml    # General configuration tasks
│   │   │   ├── macos.yml     # macOS-specific tasks
│   │   │   ├── main.yml      # Main GitHub CLI tasks
│   │   │   ├── set-gh-alias.yml # Custom alias configuration
│   │   │   └── ubuntu.yml    # Ubuntu-specific tasks
│   ├── gpg/             # GPG-related tasks
│   │   ├── tasks/       # Task files for GPG configuration
│   │   │   ├── config.yml    # General configuration tasks
│   │   │   ├── macos.yml     # macOS-specific tasks
│   │   │   ├── main.yml      # Main GPG tasks
│   │   │   └── ubuntu.yml    # Ubuntu-specific tasks
│   ├── homebrew/        # Homebrew-related tasks
│   │   ├── tasks/       # Task files for Homebrew configuration
│   │   │   ├── homebrew.yml  # Homebrew installation tasks
│   │   │   └── main.yml      # Main Homebrew tasks
│   ├── jetbrains-mono-font/ # JetBrains Mono font installation
│   │   ├── main.yml      # Main font tasks
│   │   ├── tasks/        # Task files for font installation
│   ├── ohmyzsh/         # Oh My Zsh-related tasks
│   │   ├── tasks/       # Task files for Oh My Zsh configuration
│   │   │   └── main.yml      # Main Oh My Zsh tasks
│   ├── zsh/             # Zsh-related tasks
│   │   ├── tasks/       # Task files for Zsh configuration
│   │   │   ├── macos.yml     # macOS-specific tasks
│   │   │   ├── main.yml      # Main Zsh tasks
│   │   │   └── ubuntu.yml    # Ubuntu-specific tasks
```

## CI/CD

The repository uses GitHub Actions to test the playbook on both Ubuntu and macOS environments. Tests run on:
- Every push to `master`
- Every push to feature branches (`feat/**`)
- Pull requests targeting `master` or feature branches

## Getting Started

To run the playbook locally:

```bash
ansible-playbook -i inventory.yml playbook.yml
```

You can also [use tags](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_tags.html) to run only parts of the playbook.

```bash
ansible-playbook -i inventory.yml playbook.yml --tags [TAG]
```

>You might get asked for your password as this playbook needs
>admin permissions for some tasks.

### Tag List

Here is a list with all the tags that we have in this playbook.

| Tag         | Description                                                                 |
|-------------|-----------------------------------------------------------------------------|
| `fonts`     | Installs and configures JetBrains Mono font (macOS only)                    |
| `git`       | Installs and configures Git with proper version management                  |
| `github-cli`| Installs and configures GitHub CLI with authentication and settings         |
| `gpg`       | Installs and configures GPG for secure commit signing                       |
| `homebrew`  | Installs Homebrew package manager                                           |
| `integration`| Runs integration tasks for Git, GPG, and GitHub CLI                        |
| `ohmyzsh`   | Installs and configures Oh My Zsh with plugins, themes, and aliases         |
| `zsh`       | Installs and configures Zsh with custom settings                            |

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

## Ansible Output Explanation

When running the Ansible playbook, you may encounter an output summary like the following:

```
ok=44   changed=6    unreachable=0    failed=0    skipped=28   rescued=0    ignored=0
```

Here is what each of these terms means:

- **ok**: The number of tasks that were successfully executed without making any changes. These tasks were already in the desired state.
- **changed**: The number of tasks that made changes to the system. This indicates that Ansible modified something to bring it into the desired state.
- **unreachable**: The number of hosts that Ansible could not connect to. This could be due to network issues, incorrect inventory configuration, or authentication problems.
- **failed**: The number of tasks that failed to execute successfully. Failures may require manual intervention or debugging.
- **skipped**: The number of tasks that were skipped because their conditions were not met. For example, tasks with `when` clauses that evaluated to `false`.
- **rescued**: The number of tasks that were recovered after a failure using a `rescue` block. This is part of Ansible's error handling mechanism.
- **ignored**: The number of tasks that failed but were ignored because of the `ignore_errors: yes` directive. These tasks do not stop the playbook execution.

Understanding these outputs can help you debug and optimize your playbook execution.