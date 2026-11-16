# FTP Enumeration – Anonymous Login

## 1. Lab Overview

| Category | Details |
|---|---|
| Platform | Hack The Box |
| Lab | Starting Point – Fawn |
| Target | 10.129.96.162 |
| Service | FTP |
| Port | 21/TCP |
| FTP Server | vsftpd 3.0.3 |
| Difficulty | Very Easy |
| Status | Completed |

## 2. Objective

The objective of this lab was to identify the FTP service running on the target machine, investigate its configuration, and determine whether anonymous authentication was enabled.

The exercise focused on basic network reconnaissance, service enumeration, FTP authentication and file retrieval.

## 3. Reconnaissance and Enumeration

### 3.1 Port Scanning with Nmap

I used Nmap to identify open ports and determine which services were running on the target.

**Command:**

```bash
nmap -sC -sV 10.129.96.162
```

**Explanation:**

- `-Pn`: Skip Host Discovery.
- `-sV`: Attempts to identify service versions.
- `10.129.96.162`: IP address of the target machine.

**Findings:**

The scan identified the following service:

| Port | State | Service | Version |
|---|---|---|---|
| 21/TCP | Open | FTP | vsftpd 3.0.3 |

Additional service information indicated that the target was running a Unix-like operating system.

**Analysis:**

FTP (File Transfer Protocol) is a protocol used to transfer files between a client and a server.

It normally listens on TCP port 21 for control connections. Standard FTP does not encrypt authentication credentials or data by default, which can expose sensitive information to network interception.

SFTP (SSH File Transfer Protocol) provides secure file transfer functionality over SSH.

## 4. FTP Enumeration

### 4.1 Connecting to the FTP Service

After identifying the FTP service, I used the FTP command-line client to connect to the target.

**Command:**

```bash
ftp 10.129.96.162
```

The FTP client prompted me for a username and password.

### 4.2 Anonymous Authentication

I attempted to authenticate using the username `anonymous`.

Anonymous FTP access allows users to access certain FTP resources without having a regular account on the server.

The server returned the following response:

```text
230 Login successful.
```

**Analysis:**

FTP response code `230` indicates that the user has successfully logged in.

This confirmed that anonymous FTP authentication was enabled on the target.

### 4.3 Enumerating Available Files

After successfully authenticating, I used the following command to list the available files and directories:

```bash
ls
```

The directory listing revealed a file named:

```text
flag.txt
```

This indicated that the file was accessible through the anonymous FTP session.

## 5. File Retrieval

### 5.1 Downloading the File

I used the FTP `get` command to download the discovered file to my local machine.

**Command:**

```bash
get flag.txt
```

The FTP client transferred the file from the remote server to the local working directory.

### 5.2 Reading the Downloaded File

After leaving the FTP session, I used the Linux `cat` command to display the contents of the downloaded text file.

```bash
exit
cat flag.txt
```

The file contained the flag required to complete the exercise.

## 6. Key Findings

| Finding | Description |
|---|---|
| Open FTP port | TCP port 21 was accessible |
| FTP version | vsftpd 3.0.3 |
| OS information | Unix |
| Anonymous login | Enabled |
| Accessible file | flag.txt |
| Authentication response | 230 – Login successful |

### Security Implications

Enabling anonymous FTP access can expose files to unauthorized users if access permissions are not configured properly.

In this lab, anonymous users were able to access and download a file from the FTP server.

Potential security improvements include:

- Disable anonymous FTP access if it is not required.
- Restrict file and directory permissions.
- Use SFTP or another secure file transfer mechanism.
- Monitor FTP authentication and file access.
- Avoid exposing sensitive files through publicly accessible directories.

## 7. What I Learned

Through this exercise, I gained practical experience with:

- Testing network connectivity using `ping`.
- Identifying open ports and service versions using Nmap.
- Understanding FTP and its security limitations.
- Connecting to an FTP server using the Linux command-line client.
- Testing anonymous FTP authentication.
- Interpreting FTP response codes.
- Enumerating remote directories and retrieving files.

**Main takeaway:** Service enumeration is an important first step in penetration testing. Identifying an exposed service is only the beginning; understanding its configuration and access permissions helps reveal potential security weaknesses.

---

*This exercise was performed in an authorized Hack The Box lab environment.*
