# 🔐 Linux File Permissions Lab

<div align="center">

![Linux](https://img.shields.io/badge/Linux-File%20Permissions-1793D1?logo=linux&logoColor=white)
![Security](https://img.shields.io/badge/Security-Access%20Control-4EAA25)
![chmod](https://img.shields.io/badge/Tools-chmod%20%7C%20ls%20-la-FFB000)

</div>

This project demonstrates how to review and tighten Linux access permissions to enforce the principle of least privilege. It focuses on protecting sensitive files and directories by adjusting owner, group, and other permissions using standard Unix commands such as `ls -la` and `chmod`.

## Overview

A research team needed restricted access to files in `/home/researcher2/projects`. The goal was to ensure that only authorized users could read or modify specific files while preventing unnecessary access to sensitive research data.

## Objectives

- Inspect current file and directory permissions
- Remove unauthorized access where needed
- Restrict write and execute permissions to the minimum required
- Validate the final permission state with `ls -la`

## Initial Permission State

The initial permissions were:

![Initial Permissions](images/initial-permission.png)

```text
drwx--x--- drafts
-rw-rw-rw- project_k.txt
-rw-r----- project_m.txt
-rw-rw-r-- project_r.txt
-rw-rw-r-- project_t.txt
-rw--w---- .project_x.txt
```

## Changes Applied

### 1) `project_k.txt`

Removed write access for `other` users to prevent unauthorized modification:

```bash
chmod o-w project_k.txt
```

Final permission: `-rw-rw-r--`

### 2) `project_m.txt`

Restricted access so only the owner retains access by removing the group read permission:

```bash
chmod g-r project_m.txt
```

Final permission: `-rw-------`

### 3) `.project_x.txt`

Removed write access for both the user and group, while granting read access to the group for this archived file:

```bash
chmod u-w,g-w,g+r .project_x.txt
```

Final permission: `-r--r-----`

### 4) `drafts` directory

Removed group execute permission so directory access is restricted to the owner only:

```bash
chmod g-x drafts
```

Final permission: `drwx------`

![Directory Permission Changes](images/chmod-changes.png)

## Verification

The final permission state was validated using `ls -la`:

![Final Permissions](images/final-permissions.png)

```text
drwx------ drafts
-rw-rw-r-- project_k.txt
-rw------- project_m.txt
-rw-rw-r-- project_r.txt
-rw-rw-r-- project_t.txt
-r--r----- .project_x.txt
```

## Result

The access model now follows the principle of least privilege. Sensitive files are no longer exposed to unnecessary users or groups, and the directory structure is restricted to the minimum required access level.

## Tools Used

- `ls -la` for viewing permissions
- `chmod` for modifying file and directory permissions
- Linux shell environment

## Summary

This lab shows how careful permission management can improve system security and ensure compliance with organizational policies. By limiting access to only the necessary users and groups, the environment better protects confidential research data.
