
Alejandro Fonck
11:14 p.m. (hace 0 minutos)
para mí

# Lab 02 — Linux Users, Groups, and Permissions

## Objective

Learn how Linux controls access to files and directories using users, groups, ownership, and permissions.

## Environment

- Ubuntu Server 26.04 LTS
- VMware Workstation
- Windows PowerShell
- SSH
- Bash

## What I Practiced

During this lab, I practiced:

- Identifying the current user and group memberships
- Creating and switching between Linux users
- Understanding administrative privileges with `sudo`
- Reading Linux file permissions
- Using numeric permissions with `chmod`
- Changing file ownership and group ownership
- Creating a shared group
- Controlling access to files between users
- Understanding permissions on directories
- Testing allowed and denied access

## Linux Permission Model

Linux permissions are divided into three categories:

- **Owner** — the user who owns the file
- **Group** — users who belong to the file's assigned group
- **Others** — everyone else

The basic permissions are:

- `r` = read
- `w` = write
- `x` = execute

For example:

```text
-rw-rw----
```

can be separated as:

```text
- | rw- | rw- | ---
owner group others
```

The first character describes the type of object and is not part of the three permission groups.

## Numeric Permissions

Linux permissions can also be represented numerically:

```text
r = 4
w = 2
x = 1
```

For example:

```bash
chmod 660 secret.txt
```

means:

```text
Owner: rw- = 6
Group: rw- = 6
Others: --- = 0
```

## Users and Groups

A shared group named `labteam` was used to demonstrate how multiple users can receive access to the same file without giving access to everyone on the system.

Commands such as:

```bash
id
groups
```

were used to inspect user IDs and group memberships.

## Ownership and Shared Access

The test file was configured with:

```text
-rw-rw---- 1 alejandro labteam ... secret.txt
```

This means that `alejandro` is the owner and `labteam` is the assigned group.

Because `labuser` belongs to `labteam`, it receives the group's `rw-` permissions and can read and modify the file even though it is not the owner.

## Testing Access Control

A second file was configured with:

```text
-rw------- 1 alejandro alejandro ... private.txt
```

With permission mode `600`, only the owner can read or modify the file.

When `labuser` attempted to read it, Linux returned:

```text
Permission denied
```

This demonstrated that Linux enforces access based on the user's relationship to the file and its configured permissions.

## Directory Permissions

Directory permissions behave differently from regular file permissions.

For directories:

- `r` allows listing directory contents
- `w` allows creating, deleting, or renaming entries
- `x` allows traversing the directory

A user may therefore be unable to list a directory but still access a specific file inside it if the filename is already known and the necessary permissions allow access.

## Commands Used

```bash
id
groups
su -
sudo
ls -l
ls -ld
cat
touch
chmod
chown
chgrp
groupadd
usermod
echo
pwd
```

## Screenshots

### User and Group Membership
![User and group membership](screenshots/01-user-groups.png)

### Shared Group Permissions
![Shared group permissions](screenshots/02-group-permissions.png)

### Permission Denied Test
![Permission denied](screenshots/03-permission-denied.png)

## What I Learned

This lab helped me understand that Linux permissions are not simply about whether a user is an administrator.

When a user accesses a file, Linux determines whether that user is the owner, belongs to the assigned group, or falls under others, and then applies the corresponding permissions.

I also learned how `r`, `w`, and `x` behave differently for files and directories, how numeric permissions such as `660`, `640`, and `750` are calculated, and how groups can be used to securely share resources between users.

Most importantly, I practiced testing permissions from different user accounts instead of only configuring them, which made it possible to verify that the access controls actually worked.
