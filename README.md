# DevEnv Ansible Playbook

This repository contains an Ansible playbook for setting up and maintaining a consistent development environment across macOS and Ubuntu systems.

## Purpose

The playbook automates the installation and configuration of development tools and utilities, ensuring a consistent environment across different machines and operating systems.

## Current Features

- Git installation and management:
  - Updates Apple Git via Xcode Command Line Tools on macOS
  - Installs Git via Homebrew if not present
  - Provides status information about the installed Git version

- GitHub CLI installation:
  - Installs GitHub CLI (gh) via Homebrew on macOS
  - Installs GitHub CLI via apt on Ubuntu
  - Provides installation status and authentication instructions

## Repository Structure

```
.
├── .github/workflows/   # GitHub Actions workflow configurations
├── inventory.yml       # Ansible inventory file
└── playbook.yml       # Main Ansible playbook
```

## CI/CD

The repository uses GitHub Actions to test the playbook on both Ubuntu and macOS environments. Tests run on:
- Every push to master
- Every push to feature branches (feat/**)
- Pull requests targeting master or feature branches

## For AI Agents

This repository is AI-friendly and follows these conventions:

### Commit Messages
- Uses semantic commit messages (feat:, fix:, ci:, etc.)
- Each commit message has a clear title and description
- Descriptions use bullet points for multiple items

### Branch Strategy
- Main branch: `master`
- Feature branches: `feat/**`
- CI runs on all branches to ensure early feedback

### Development Workflow
1. Create a feature branch from master
2. Make changes and test locally
3. Push changes and create a PR
4. Merge to master after CI passes

## Getting Started

To run the playbook locally:

```bash
ansible-playbook -i inventory.yml playbook.yml
```

## Requirements

- Python 3.x
- Ansible
- For macOS: Homebrew (will be installed if missing) 