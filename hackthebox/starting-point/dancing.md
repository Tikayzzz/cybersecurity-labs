# SMB Enumeration – Dancing

## 1. Lab Overview

| Category | Details |
|---|---|
| Platform | Hack The Box |
| Lab | Starting Point – Dancing |
| Target | 10.129.96.153 |
| Service | SMB (Server Message Block) |
| Port | 445/TCP |
| Service Name | microsoft-ds |
| Tool | Nmap, smbclient |
| Difficulty | Easy |
| Status | Completed |

## 2. Objective

The objective of this lab was to identify the SMB service running on the target machine, enumerate available network shares, and investigate whether any shares could be accessed without providing authentication credentials.

The exercise focused on network reconnaissance, SMB enumeration, anonymous access and remote file retrieval.

## 3. Reconnaissance and Enumeration

### 3.1 Initial Nmap Scan

I started by scanning the target IP address using Nmap to identify available services and their versions.

**Command:**

```bash
nmap -Pn -sV 10.129.96.153
```

**Explanation:**

- `-Pn`: Skips host discovery and treats the target as online.
- `-sV`: Attempts to identify the versions of services running on open ports.
- `<TARGET_IP>`: IP address of the target machine.

**Findings:**

The scan identified SMB running on TCP port 445.

| Port | State | Service |
|---|---|---|
| 445/TCP | Open | microsoft-ds |

**Analysis:**

SMB stands for Server Message Block. It is a network communication protocol commonly used for sharing files, directories and other resources between devices.

SMB commonly operates over TCP port 445.

The service name `microsoft-ds` is commonly associated with SMB over TCP port 445.

## 4. SMB Share Enumeration

### 4.1 Listing Available Shares

After identifying the SMB service, I used the `smbclient` utility to enumerate the available network shares.

**Command:**

```bash
smbclient -N -L 10.129.96.153
```

**Explanation:**

- `smbclient`: Command-line utility for interacting with SMB servers.
- `-N`: Attempts to connect without requesting a password.
- `-L`: Lists the available shares on the remote SMB server.
- `10.129.96.153`: Specifies the target SMB server.

**Findings:**

The enumeration revealed four available SMB shares.

The share that I was ultimately able to access without providing a password was:

```text
WorkShares
```

**Analysis:**

The successful enumeration demonstrated that the SMB server allowed share discovery without providing a password.

However, the ability to list shares does not automatically mean that every share is accessible. Each share can have different access permissions.

## 5. Accessing the SMB Share

### 5.1 Connecting Without a Password

I attempted to access the `WorkShares` share using `smbclient` without providing a password.

**Command:**

```bash
smbclient -N \\\\10.129.96.153\\10.129.96.153
```

The connection was successful, allowing me to interact with the remote SMB share through an interactive shell.

### 5.2 Enumerating Files and Directories

Once connected, I used the following command to list the available files and directories:

```bash
ls
```

The directory listing revealed a directory named:

```text
James.P
```

I navigated into this directory to investigate its contents.

```bash
cd James.P
ls
```

The directory contained the flag file.

## 6. File Retrieval

### 6.1 Downloading the Flag

Within the SMB shell, I used the `get` command to download the discovered file to my local machine.

**Command:**

```bash
get flag.txt
```

The file was transferred from the remote SMB share to my local working directory.

### 6.2 Reading the Downloaded File

After leaving the SMB session, I used the Linux `cat` command to read the downloaded file.

```bash
exit
cat flag.txt
```

The file contained the flag required to complete the exercise.

## 7. Key Findings

| Finding | Description |
|---|---|
| Open port | TCP 445 |
| Service | microsoft-ds |
| Protocol | SMB |
| Available shares | 4 |
| Accessible share | WorkShares |
| Authentication | No password required |
| Discovered directory | james.P |
| File retrieval | Successful using `get` |

### Security Implications

Allowing unauthenticated users to access SMB shares can expose sensitive information and files.

In this exercise, the `WorkShares` share was accessible without a password, and a user was able to retrieve a file from the share.

Potential security improvements include:

- Restrict anonymous and guest access.
- Apply least-privilege permissions to SMB shares.
- Require appropriate authentication.
- Monitor access to shared resources.
- Avoid storing sensitive files in publicly accessible shares.

## 8. What I Learned

Through this exercise, I gained practical experience with:

- Identifying SMB services using Nmap.
- Understanding SMB and TCP port 445.
- Enumerating network shares using `smbclient`.
- Understanding the `-N` and `-L` options.
- Connecting to SMB shares without providing a password.
- Navigating remote directories.
- Downloading files using the SMB interactive shell.

**Main takeaway:** Enumerating an exposed service is not enough. Understanding its authentication mechanisms and access permissions is essential for identifying potential security weaknesses.

---

*This exercise was performed in an authorized Hack The Box lab environment.*
