# Fix-Repository-Issues-CentOS-7-EOL
How to Fix Repository Issues on CentOS 7 After End-of-Life (EOL)

## Introduction

If you're using a **minimal installation of CentOS 7**, you might face issues when trying to install or update packages. This happens because the default CentOS 7 repositories are no longer supported.

The solution is to use the **CentOS Vault Repository** at `http://vault.centos.org`. This repository contains older versions of CentOS, including the last release of CentOS 7 (`7.9.2009`).

This guide will show you how to configure the repository for a minimal CentOS 7 installation and ensure that you can update and install essential tools like `nano`, `wget`, or `vim`.

## Steps to Configure the Repository

### 1. Check Your CentOS Version

First, verify that your CentOS version is `7.9.2009`. You can check the version with the following command:

```bash
cat /etc/redhat-release
```
If it shows something like CentOS Linux release 7.9.2009, you can proceed with the configuration.

### 2. Backup the Default Repository Configuration
Before making any changes, it's important to back up the current repository configuration file. This will allow you to restore it if necessary.

Run the following command to create a backup of the repository file:
```bash
cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

### 3. Add the CentOS Vault Repository
Now, you will create a new .repo file for the CentOS Vault repository. Since you are using a minimal installation, you can use the vi editor (or nano if it's available) to create this new repository file.

Run the following command:

```bash
vi /etc/yum.repos.d/CentOS-Vault.repo
```

Then, add the following content to the file:

```bash
# /etc/yum.repos.d/CentOS-Vault.repo

[base]
name=CentOS-7.9.2009 - Base
baseurl=http://vault.centos.org/7.9.2009/os/$basearch/
gpgcheck=1
enabled=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

[updates]
name=CentOS-7.9.2009 - Updates
baseurl=http://vault.centos.org/7.9.2009/updates/$basearch/
gpgcheck=1
enabled=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

[extras]
name=CentOS-7.9.2009 - Extras
baseurl=http://vault.centos.org/7.9.2009/extras/$basearch/
gpgcheck=1
enabled=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

[centosplus]
name=CentOS-7.9.2009 - Plus
baseurl=http://vault.centos.org/7.9.2009/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
```

After adding the content, save and exit the file `(esc -> :wq -> enter)` in vi.

### 4. Clean the YUM Cache
After adding the new repository configuration, clean the YUM cache to ensure the new repository settings are applied correctly:

```bash
yum clean all
```

### 5. Run `yum update`
Once you have configured the new repository, it is important to update your system to ensure that you have the latest packages available from the CentOS Vault repository.

Run the following command to update all installed packages:

```bash
yum update -y
```

This will:
-	Check for updates for all installed packages.
-	Download and install any available updates, ensuring your system is up to date with the latest patches.

### 6. Test the Configuration
Now, check that the repositories are set up correctly by running:

```bash
yum repolist
```

You should see the `base`, `updates`, and `extras` repositories listed, all pointing to `vault.centos.org`.

### 7. Install Essential Tools
Since this is a minimal installation, you may not have tools like nano or wget. You can install them using the following command:

```bash
yum install nano wget -y
```

### 8. Enable Additional Repositories (Optional)
If you need additional repositories like `centosplus`, you can enable them by setting `enabled=1` in the configuration file.

## Tips and Notes
-	Always ensure you are pointing to `7.9.2009`, as it is the last supported version of `CentOS 7`.
-	If you need other repositories, enable them by modifying `enabled=0` to `enabled=1` in the `.repo` file.
-	Minimal installations often require extra dependencies. Use `yum` to install additional packages as needed.

## Conclusion
By following these steps, you will be able to continue using CentOS 7 with the CentOS Vault repository, even though official support has ended. This configuration will allow you to install and update packages without issues.
