# Managing Multiple GitHub Accounts on Mac and Ubuntu

![git](https://github.com/user-attachments/assets/5e7d5876-7a0c-4e39-80ef-2bf4663fc520)


This guide walks you through configuring and managing multiple GitHub accounts on a Mac or Ubuntu system. It’s suitable for users needing separate SSH keys for personal and work-related GitHub repositories. 

## Prerequisites

- Git installed on your system.
- Basic knowledge of the terminal and SSH key management.

---

## Steps

### 1. Create an SSH Key Pair for Each Account

Generate separate SSH keys for your personal and work accounts.

1. **Create an SSH key for your personal account:**

   ```bash
   ssh-keygen -t rsa -C "your-personal-email@gmail.com"
   ```

   - When prompted, specify a unique file name for the key (e.g., `id_rsa_personal`):
   
     ```bash
     Enter file in which to save the key (/home/user/.ssh/id_rsa): /home/user/.ssh/id_rsa_personal
     ```

2. **Create an SSH key for your company account:**

   ```bash
   ssh-keygen -t rsa -C "your-company-email@gmail.com"
   ```

   - Specify another unique file name (e.g., `id_rsa_company`):
   
     ```bash
     Enter file in which to save the key (/home/user/.ssh/id_rsa): /home/user/.ssh/id_rsa_company
     ```

### 2. Add SSH Keys to the SSH Agent

1. Start the SSH agent in the background:

   ```bash
   eval "$(ssh-agent -s)"
   ```

2. Add each key to the SSH agent:

   ```bash
   ssh-add ~/.ssh/id_rsa_personal
   ssh-add ~/.ssh/id_rsa_company
   ```

### 3. Configure the SSH Config File

1. Open the SSH config file (create one if it doesn’t exist):

   ```bash
   nano ~/.ssh/config
   ```

2. Add configurations for each account. This enables you to specify which key to use for different GitHub repositories.

   ```plaintext
   # Personal GitHub account
   Host github.com-personal
       HostName github.com
       User git
       IdentityFile ~/.ssh/id_rsa_personal

   # Company GitHub account
   Host github.com-company
       HostName github.com
       User git
       IdentityFile ~/.ssh/id_rsa_company
   ```

3. Save and close the file.

### 4. Add SSH Keys to GitHub

1. Copy each SSH public key:

   ```bash
   cat ~/.ssh/id_rsa_personal.pub
   cat ~/.ssh/id_rsa_company.pub
   ```

2. Go to **GitHub > Settings > SSH and GPG keys** and add each key separately under your personal and company accounts.

### 5. Clone Repositories Using Specific SSH Configurations

To ensure the correct SSH key is used, clone each repository using the custom hostnames from your `~/.ssh/config` file.

1. **Personal repository:**

   ```bash
   git clone git@github.com-personal:YourUsername/your-personal-repo.git
   ```

2. **Company repository:**

   ```bash
   git clone git@github.com-company:YourCompany/your-company-repo.git
   ```

### 6. Configure Git User for Each Repository

To avoid mix-ups, set the Git username and email for each repository.

1. Navigate to the repository directory:

   ```bash
   cd your-personal-repo/
   ```

2. Configure Git user details:

   ```bash
   git config user.name "Your Personal Name"
   git config user.email "your-personal-email@gmail.com"
   ```

3. Repeat these steps for the company repository:

   ```bash
   cd your-company-repo/
   git config user.name "Your Company Name"
   git config user.email "your-company-email@gmail.com"
   ```

---

## Verification

To verify the correct key and GitHub account are in use:

1. **List added SSH identities:**

   ```bash
   ssh-add -l
   ```

2. **Test the SSH connection for each account:**

   ```bash
   ssh -T github.com-personal
   ssh -T github.com-company
   ```

   You should see a success message for each account, confirming the correct SSH key is being used.

---

This guide provides a structured approach to manage multiple GitHub accounts efficiently. Follow each step to configure and verify account-specific SSH connections for your repositories.
