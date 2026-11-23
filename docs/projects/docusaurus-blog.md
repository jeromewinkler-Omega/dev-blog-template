---
sidebar_position: 1
---

# Docusaurus-Blog


A personal documentation and blog site built with Docusaurus. This guide walks through the complete setup — from creating an SSH key to running the site locally — and the feature-branch workflow used to submit changes via pull request.

## Table of content

- [Prerequesites](#prerequesites)
- [Quickstart](#quickstart)
- [Description](#description)
- [Further References](#further-references)

## Prerequesites

to start your own Docusaurus you have to minimal install and set up the following:

- Windows 10/11 with PowerShell and the built-in OpenSSH client
- [VS-Code](https://code.visualstudio.com/)
- your own [GitHub](https://github.com/) account 
- [node.js](Version see in your Docusaurus/package.json)
- a [Docusaurus](https://docusaurus.io/) template


## Features 

- **Documentation & blog** where you can store all your static Guides and knowledge base (Container,Git,one writeups and more)
- **Security:** Integrity scan through your package.json. Secrets live only in the local `../../.env` file, which is excluded via `.gitignore`.
- **Markdown and MDX Support:** You write content in standard Markdown while embedding interactive JSX and React components directly inside your pages via Docusaurus Markdown Features.
- **Pluggable Architecture:** The core system delegates all features, routes, and capabilities to individual plugins. You can start with a basic template and add official or community plugins as your needs grow.

## Quickstart

1. **Configurate Git** with your own identity credentials [step 1](#1--configure-git-one-time-setup)
2. **Create a SSH-Key** and add it to your GitHub-Settings [step2](#2-create-an-ssh-key-and-add-it-to-github-one-time-setup-per-pc)
3. **Test the SSH connection** [step 3](#3-to-test-the-ssh-connection)
4. **Create a repository** and clone the template [step 4](#4-create-your-own-repository-from-the-template-and-clone-it)
5. **Create a branch** to make your changes -> `git switch -c <branchname>` -> [setp 5](#5create-a-feature-branch)
6. **Install dependencies:** `npm install` or `npm ci` [step 6](#6-install-dependencies)
7. **Create your local `../../.env` file** -> [step 7](#7-create-a-env-file-if-a-envexample-file-exists-in-the-repo)
8. **start the dev-server localy:** -> `npm run start` [step 8](#8-develop-build-and-test)
9. **Commit**, push and open a pull request to merge new features. `git add <modifed files>` `git commit -m "<changes>"` `git push` -> [step 9](#9-submit-changes)
10. [Further References](#further-references)

### 1.  Configure Git (one-time setup)



```
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

### 2. Create an SSH key and add it to GitHub (one-time setup per PC)

Create the key. Confirm the file location by pressing Enter. Setting a passphrase is recommended but not necessary.



```
ssh-keygen -t ed25519 -C "your@email.com"
```

Confirm the file location by pressing Enter. 

Setting a passphrase is recommended but not neccessary.

Next, start the SSH agent so you don’t have to enter your passphrase every time. 


```powershell
Get-Service ssh-agent | Set-Service -StartupType Automatic
Start-Service ssh-agent
ssh-add $HOME\.ssh\id_ed25519
```
On Windows, the agent stores the key permanently, so `ssh-add` only needs to be run once.
Then, copy the public key to your clipboard:

Powershell:
```powershell
Get-Content $HOME\.ssh\id_ed25519.pub 
```

On GitHub, add it under Settings → SSH and GPG keys → New SSH key.


```
git config --global core.sshCommand "C:/Windows/System32/OpenSSH/ssh.exe"
```
### 3. To test the SSH connection:

```
ssh -T git@github.com
```

:::note
If you see "Hi yourname! You’ve successfully authenticated," it is working correctly.
:::

### 4. Create your own repository from the template and clone it

Open the template repository in your browser, click **Use this template → Create a new repository**, then clone it:

```
cd C:\Directory
git clone https://github.com/jeromewinkler-Omega/dev-blog-template.git
cd project
```

 ### 5.Create a feature branch:

```
git switch -c feature/setup-blog
```
```
git switch main
git pull origin main 
```


### 6. Install dependencies

```
npm install
#or
npm ci
```

:::note
`npm ci` installs the exact versions from `../../package-lock.json`. Use `npm install <package>` only when adding a new package.
:::

```
# if necessayry update your react-/ react-dom verion 
npm install react@19 react-dom@19 
npm run build
```


### 7. Create a .env file (if a .env.example file exists in the repo):

Only needed if the repository contains a `.env.example` file:
```
Copy-Item .env.example .env
```
Then, enter your values into the .env file.
needs to be excluded via `gitignore` and is not allowed to commit!

## Usage 

### 8. Develop, build, and test



```
npm run start      # Development with live-reload, localhost:3000
npm run build      # Build the final site 
into the 'build' directory
npm run serve      # Test the build; press Ctrl + C to exit
```

### 9. Submit changes



```
git add .
git commit -m "Setup blog"
git push -u origin feature/setup-blog
gh pr create --base main
```

If the CI check passes (turns green), merge the PR into the 'main' branch.
________________________________________
Steps 0 through 3 only need to be performed once per computer. For each new project, start from step 4.

## Description
This project is a static documentation and blog site built with [Docusaurus 3](https://docusaurus.io/), based on the Developer Akademie DevSecOps course template. It contains guides, a knowledge base (Containers, DevOps, Git, Linux) and project write-ups, and is available in English.
 
Key aspects of the setup:

- **Security:** `../../package-lock.json` stores an integrity hash (SHA-512) for each package, so tampered downloads are detected. Secrets live only in the local `.env` file, which is excluded via `.gitignore`.
- **Feature-branch workflow:** All changes are made in feature branches and merged into `main` through a pull request, which serves as the review and approval step.
- **CI/CD:** A GitHub Actions workflow builds the site on every pull request and deploys it to GitHub Pages after merging into `main`.
- **Dependency updates:** Dependabot opens pull requests for outdated packages. Each update is only merged if the CI build passes.


## Further References

- [Docusaurus Documentation](https://docusaurus.io/docs)
- [Docusaurus Deployment](https://docusaurus.io/docs/deployment)
- [GitHub Docs: Connecting to GitHub with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)
- [GitHub Docs: Creating a repository from a template](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Dependabot Documentation](https://docs.github.com/en/code-security/dependabot)
- [GitHub CLI Manual](https://cli.github.com/manual/)
- [npm ci](https://docs.npmjs.com/cli/commands/npm-ci)
- [Git Documentation](https://git-scm.com/doc)---
