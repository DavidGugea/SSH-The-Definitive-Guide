# SSH - The Secure Shell - The Definitive Guide

## 1. Introduction to SSH
## 2. Basic Client Use
## 3. Inside SSH
## 4. Installation and Compile-Time Configuration
## 5. Serverwide Configuration
## 6. Key Management and Agents
## 7. Advanced Client Use
## 8. Per-Account Server Configuration
## 9. Port Forwarding and X Forwarding
## 10. A Recommended Setup
## 11. Case Studies
## 12. Troubleshooting and FAQ
## 13. Overview of Other Implementations
## 14. OpenSSH for Windows
## 15. OpenSSH for Macintosh
## 16. Tectia for Windows
## 17. SecureCRT and SecureFX for Windows
## 18. PuTTY for Windows

---
---

# 1. Introduction to SSH

## What is SSH ?

SSH, the Secure Shell, is a popular, powerful, software-based approach to network security.* Whenever data is sent by a computer to the network, SSH automatically encrypts (scrambles) it. Then, when the data reaches its intended recipient, SSH automatically decrypts (unscrambles) it. The result is transparent encryption: users can work normally, unaware that their communications are safely encrypted on the network. In addition, SSH uses modern, secure encryption algorithms and is effective enough to be found within mission-critical applications at major corporations.

SSH has a client/server architecture, as shown in Figure 1-1. An SSH server program, typically installed and run by a system administrator, accepts or rejects incoming connections to its host computer. Users then run SSH client programs, typically on other computers, to make requests of the SSH server, such as “Please log me in,” “Please send me a file,” or “Please execute this command.” All communications between clients and servers are securely encrypted and protected from modification.

![SSH Architecture](ScreenshotsForNotes/Chapter1/SSHArchitecture.PNG)

Our description is simplified but should give you a general idea of what SSH does. We’ll go into depth later. For now, just remember that SSH clients communicate with SSH servers over encrypted network connections.

Many SSH clients are inspired by old Unix programs called the “r-commands:” rsh (remote shell), rlogin (remote login), and rcp (remote copy). In fact, for many purposes the SSH clients are drop-in replacements for the r-commands, so if you’re still using them, switch to SSH immediately! The old r-commands are notoriously insecure, and the SSH learning curve is small.

## What SSH is not ?

Although SSH stands for Secure Shell, it is not a true shell in the sense of the Unix Bourne shell and C shell. It is not a command interpreter, nor does it provide wildcard expansion, command history, and so forth. Rather, SSH creates a channel for running a shell on a remote computer, with end-to-end encryption between the two systems.

SSH is also not a complete security solution—but then, nothing is. It won’t protect computers from active break-in attempts or denial-of-service attacks, and it won’t eliminate other hazards such as viruses, Trojan horses, and coffee spills. It does, however, provide robust and user-friendly encryption and authentication.

## The SSH Protocol

SSH is a protocol, not a product. It is a specification of how to conduct secure communication over a network. Although we say “the SSH protocol,” there are actually two incompatible versions of the protocols in common use: SSH-1 (a.k.a. SSH-1.5) and SSH-2. We distinguish these protocols later.

The SSH protocol covers authentication, encryption, and the integrity of data transmitted over a network, as shown in Figure 1-2. Let’s define these terms:

* Authentication
    * Reliably determines someone’s identity. If you try to log into an account on a remote computer, SSH asks for digital proof of your identity. If you pass the test, you may log in; otherwise, SSH rejects the connection.
* Encryption
    * Scrambles data so that it is unintelligible except to the intended recipients. This protects your data as it passes over the network.
* Integrity
    * Guarantees the data traveling over the network arrives unaltered. If a third party captures and modifies your data in transit, SSH detects this fact.

![Authentication Encryption And Integrity](ScreenshotsForNotes/Chapter1/AuthenticationEncryptionAndIntegrity.PNG)

In short, SSH makes network connections between computers, with strong guarantees that the parties on both ends of the connection are genuine. It also ensures that any data passing over these connections arrives unmodified and unread by eavesdroppers.

## Protocols, Products, Clients, and Confusion

The first SSH product, created by Tatu Ylönen for Unix, was simply called “SSH.” This caused confusion because SSH was also the name of the protocol. In this book, we use more precise terminology to refer to protocols, products, and programs, summarized in the sidebar “Terminology: SSH Protocols and Products.” In short:

* Protocols are denoted with dashes: SSH-1, SSH-2.
*  Products are denoted in mixed case, without dashes: OpenSSH, Tectia, PuTTY, etc.
* Client programs are in lowercase: ssh, scp, putty, etc.

## Terminology: SSH Protocols and Products

* SSH
    * A generic term referring to SSH protocols and software products.
* SSH-1
    * The SSH protocol, Version 1. This is the original protocol, and it has serious limitations, so we do not recommend its use anymore.
* SSH-2
    * The SSH protocol, Version 2, the most common and secure SSH protocol used today. It is defined by draft standards documents of the IETF SECSH working group. [3.4]
* SSH1
    * The granddaddy of it all: the original SSH product created by Tatu Ylönen. It implemented (and defined) the SSH-1 protocol and is now obsolete.
* SSH2
    * The original SSH-2 product, created by Tatu Ylönen and his company, SSH Communications
* ssh (all lowercase letters)
    * A client program run on the command line and included in many SSH products, for running secure terminal sessions and remote commands. On some systems it might be named ssh1 or ssh2.
* OpenSSH
    * The product OpenSSH from the OpenBSD project, http://www.openssh.com.
* Tectia
    * The successor to SSH2, this refers to the product suite “SSH Tectia” from SSH Communications Security. We abbreviate the name as simply “Tectia.” Since Tectia is available for both Unix and Windows, when we write “Tectia” we generally mean the Unix version unless we say otherwise.

## Overview of SSH Features

### Secure Remote Logins

Suppose you have login accounts on several computers on the Internet. Common programs like telnet let you log into one computer from another, say, from your home PC to your web hosting provider, or from one office computer to another. Unfortunately, telnet and similar programs transmit your username and password in plain text over the Internet, where a malicious third party can intercept them. Additionally, your entire telnet session is readable by a network snooper.

SSH completely avoids these problems. Rather than running the insecure telnet program, you run the SSH client program ssh. To log into an account with the username smith on the remote computer host.example.com, use this command:

```bash
$ ssh -l smith host.example.com
```

The client authenticates you to the remote computer’s SSH server using an encrypted connection, meaning that your username and password are encrypted before they leave the local machine. The SSH server then logs you in, and your entire login session is encrypted as it travels between client and server. Because the encryption is transparent, you won’t notice any differences between telnet and the telnet-like SSH client.

### Terminology: Networking

* Local computer (local host, local machine)
* A computer on which you are logged in and, typically, running an SSH client.
* Remote computer (remote host, remote machine)
* A second computer you connect to via your local computer. Typically, the remote computer is running an SSH server and is accessed via an SSH client. As a degenerate case, the local and remote computers can be the same machine.
* Local user
* A user logged into a local computer.
* Remote user
* A user logged into a remote computer.
* Server
* An SSH server program.
* Server machine
* A computer running an SSH server program. We sometimes simply write “server” for the server machine when the context makes clear (or irrelevant) the distinction between the running SSH server program and its host machine.
* Client
* An SSH client program.
* Client machine
* A computer running an SSH client. As with the server terminology, we simply write “client” when the context makes the meaning clear.
* ~ or $HOME
* A user’s home directory on a Unix machine, particularly when used in a file path such as ~/filename. Most shells recognize ~ as a user’s home directory, with the notable exception of the Bourne shell. $HOME is recognized by all shells.

### Secure File Transfer

Suppose you have accounts on two Internet computers, me@firstaccount.com and metoo@secondaccount.com, and you want to transfer a file from the first to the second account. The file contains trade secrets about your business, however, that must be kept from prying eyes. A traditional file-transfer program, such as ftp, doesn’t provide a secure solution. A third party can intercept and read the packets as they travel over the network. To get around this problem, you can encrypt the file on firstaccount.com with a program such as Pretty Good Privacy (PGP), transfer it via traditional means, and decrypt the file on secondaccount.com, but such a process is tedious and nontransparent to the user.

Using SSH, the file can be transferred securely between machines with a single secure copy command. If the file were named myfile, the command executed on firstaccount.com might be:

```bash
$ scp myfile metoo@secondaccount.com:
```

When transmitted by scp, the file is automatically encrypted as it leaves firstaccount. com and decrypted as it arrives on secondaccount.com.

### Secure Remote Command Execution

Suppose you are a system administrator who needs to run the same command on many computers. You’d like to view the active processes for each user on four different computers—grape, lemon, kiwi, and melon—on a local area network using the Unix command /usr/bin/w. Many SSH clients can run a single remote command if you provide it at the end of the command line. This short shell script does the trick:

```bash
#!/bin/sh
for machine in grape lemon kiwi melon
do
    ssh $machine /usr/bin/w Execute remote command by ssh
done
```

Each w command and its results are encrypted as they travel across the network, and strong authentication techniques may be used when connecting to the remote machines.

### Key and Agents

Suppose you have accounts on many computers on a network. For security reasons, you prefer different passwords on all accounts; but remembering so many passwords is difficult. It’s also a security problem in itself. The more often you type a password, the more likely you’ll mistakenly type it in the wrong place. (Have you ever accidentally typed your password instead of your username, visible to the world? Ouch! And on many systems, such mistakes are recorded in a system log file, revealing your password in plain text.) Wouldn’t it be great to identify yourself only once and get secure access to all the accounts without continually typing passwords?

SSH has various authentication mechanisms, and the most secure is based on keys rather than passwords. Keys are discussed in great detail in Chapter 6, but for now we define a key as a small blob of bits that uniquely identifies an SSH user. For security, a key is kept encrypted; it may be used only after entering a secret passphrase to decrypt it.

Using keys, together with a program called an authentication agent, SSH can authenticate you to all your computer accounts securely without requiring you to memorize many passwords or enter them repeatedly. It works like this:

1. In advance (and only once), place special, nonsecure files called public key files into your remote computer accounts. These enable your SSH clients (ssh, scp) to access your remote accounts.
2. On your local machine, invoke the ssh-agent program, which runs in the background.
3. Choose the key (or keys) you will need during your login session.
4. Load the keys into the agent with the ssh-add program. This requires knowledge of each key’s secret passphrase.

At this point, you have an ssh-agent program running on your local machine, holding your secret keys in memory. You’re now done. You have passwordless access to all your remote accounts that contain your public key files. Say goodbye to the tedium of retyping passwords! The setup lasts until you log out from the local machine or terminate ssh-agent.

### Access Control

Suppose you want to permit another person to use your computer account, but only for certain purposes. For example, while you’re out of town you’d like your secretary to read your email but not to do anything else in your account. With SSH, you can give your secretary access to your account without revealing or changing your password, and with only the ability to run the email program. No system-administrator privileges are required to set up this restricted access. (This topic is the focus of Chapter 8.)

### Port Forwarding

SSH can increase the security of other TCP/IP-based applications such as telnet, ftp, and the X Window System. A technique called port forwarding or tunneling reroutesa TCP/IP connection to pass through an SSH connection, transparently encrypting it end to end. Port forwarding can also pass such applications through network firewalls that otherwise prevent their use.

Suppose you are logged into a machine away from work and want to access the internal news server at your office, news.yoyodyne.com. The Yoyodyne network is connected to the Internet, but a network firewall blocks incoming connections to most ports, particularly port 119, the news port. The firewall does allow incoming SSH connections, however, since the SSH protocol is secure enough that even Yoyodyne’s rabidly paranoid system administrators trust it. SSH can establish a secure tunnel on an arbitrary local TCP port—say, port 3002—to the news port on the remote host. The command might look a bit cryptic at this early stage, but here it is:

```bash
$ ssh -L 3002:localhost:119 news.yoyodyne.com
```

This says “ssh, please establish a secure connection from TCP port 3002 on my local machine to TCP port 119, the news port, on news.yoyodyne.com.” So, in order to read news securely, configure your news-reading program to connect to port 3002 on your local machine. The secure tunnel created by ssh automatically communicates with the news server on news.yoyodyne.com, and the news traffic passing through the tunnel is protected by encryption.

## Pretty Good Privacy (PGP) and GNU Privacy Guard (GnuPG)

PGP is a popular encryption program available for many computing platforms, created by Phil Zimmerman. It can authenticate users and encrypt data files and email messages. GnuPG is a more powerful successor to PGP with less-restrictive licensing.

SSH incorporates some of the same encryption algorithms as PGP and GnuPG, but applied in a different way. PGP is file-based, typically encrypting one file or email message at a time on a single computer. SSH, in contrast, encrypts an ongoing session between networked computers. The difference between PGP and SSH is like that between a batch job and an interactive process.

PGP and SSH are related in another way as well: Tectia can optionally use PGP keys for authentication