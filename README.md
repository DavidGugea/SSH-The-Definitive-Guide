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

# 2. Basic Client Use

## A Running Example

Suppose you’re out of town on a business trip and want to access your files, which sit on a Unix machine belonging to your ISP, shell.isp.com. A friend at a nearby university agrees to let you log into her Linux account on the machine local.university. edu, and then remotely log into yours. For the remote login you could use the telnet program, but as we’ve seen, this connection between the machines is insecure. (No doubt some subversive college student would grab your password and turn your account into a renegade web server for pirated software and death metal MP3s.) Fortunately, both your friend’s machine and your ISP’s have an SSH product installed. In the example running through the chapter, we represent the shell prompt of the local machine, local.university.edu, as a dollar sign ($) and the prompt on shell.isp.com as shell.isp.com>.

## Remote Terminal Sessions wit hssh

Suppose your remote username on shell.isp.com is pat. To connect to your remote account from your friend’s account on local.university.edu, you type:

```bash
$ ssh -l pat shell.isp.com
pat's password: ******
Last login: Mon Aug 16 19:32:51 2004 from quondam.nefertiti.org
You have new mail.
shell.isp.com>
```

This leads to the situation shown in Figure 2-1. The ssh command runs a client that contacts the SSH server on shell.isp.com over the Internet, asking to be logged into the remote account with username pat. You can also provide user@host syntax instead of the –l option to accomplish the same thing:

```bash
ssh pat@shell.isp.com
```

![Figure 2-1](ScreenshotsForNotes/Chapter2/Figure_2_1.PNG)

On first contact, SSH establishes a secure channel between the client and the server so that all transmissions between them are encrypted. The client then prompts for your password, which it supplies to the server over the secure channel. The server authenticates you by checking that the password is correct and permits the login. All subsequent client/server exchanges are protected by that secure channel, including everything you type into the SSH application and everything it displays to you from shell.isp.com.

It’s important to remember that the secure channel exists only between the SSH client and server machines. After logging into shell.isp.com via ssh, if you then telnet or ftp to a third machine, insecure.isp.com, the connection between shell.isp.com and insecure.isp.com is not secure. However, you can run another ssh client from shell.isp. com to insecure.isp.com, creating another secure channel, which keeps the chain of connections secure.

## File Transfers with scp

Continuing the story, suppose that while browsing your files, you encounter a PDF file you’d like to print. In order to send the file to a local printer at the university, you must first transfer the file to local.university.edu. Once again, you reject as insecure the traditional file-transfer programs, such as ftp. Instead, you use another SSH client program, scp, to copy the file across the network via a secure channel.

First, you write the attachment to a file in your home directory on shell.isp.com using your mail client, naming the file printme.pdf. When you’ve finished reading your other email messages, log out of shell.isp.com, ending the SSH session and returning to the shell prompt on local.university.edu. You’re now ready to copy the file securely.

The scp program has syntax much like the traditional Unix cp program for copying files.* It is roughly:

```bash
$ scp name-of-source name-of-destination
```

In this example, scp copies the file printme.pdf on shell.isp.com over the network to a local file in your friend’s account on local.university.edu, also called printme.pdf:

```bash
$ scp pat@shell.isp.com:printme.pdf printme.pdf
```

The file is transferred over an SSH-secured connection. The source and destination files may be specified not only by filename, but also by username (“pat” in our example) and hostname (shell.isp.com), indicating the location of the file on the network. Depending on your needs, various parts of the source or destination name can be omitted, and default values used. For example, omitting the username and the at sign (pat@) makes scp assume that the remote username is the same as the local one.

Like ssh, scp prompts for your remote password and passes it to the SSH server for verification. If successful, scp logs into the pat account on shell.isp.com, copies your remote file printme.pdf to the local file printme.pdf, and logs out of shell.isp.com. The local file printme.pdf may now be sent to a printer.

The destination filename need not be the same as the remote one. For example, if you’re feeling French, you could call the local file imprime-moi.pdf:

```bash
$ scp pat@shell.isp.com:printme.pdf imprime-moi.pdfj
```

The full syntax of scp can represent local and remote files in powerful ways, and the program also has numerous command-line options.

## Known Hosts

The first time an SSH client encounters a new remote machine, it may report that it’s never seen the machine before, printing a message like the following:

```bash
$ ssh -l pat shell.isp.com
The authenticity of host 'shell.isp.com (192.168.0.2)' can't be established.
RSA key fingerprint is 77:a5:69:81:9b:eb:40:76:7b:13:04:a9:6c:f4:9c:5d.
Are you sure you want to continue connecting (yes/no)?
```

Assuming you respond yes (the most common response), the client continues:

> Warning: Permanently added 'shell.isp.com,192.168.0.2' (RSA) to the list of known hosts.

This message appears only the first time you contact a particular remote host. The message is a security feature related to SSH’s concept of known hosts. (Depending on your client configuration, ssh might print a different message and automatically accept or reject the connection.)

Suppose an adversary wants to obtain your password. He knows you are using SSH, and so he can’t monitor your connection by eavesdropping on the network. Instead, he subverts the naming service used by your local host so that the name of your intended remote host, shell.isp.com, translates falsely to the IP address of a computer run by him! He then installs an altered SSH server on the phony remote host and waits. When you log in via your trusty SSH client, the altered SSH server records your password for the adversary’s later use (or misuse, more likely). The bogus server can then disconnect with a preplanned error message such as “System down for maintenance—please try again after 4:00 p.m.” Even worse, it can fool you completely by using your password to log into the real shell.isp.com and transparently pass information back and forth between you and the server, monitoring your entire session. This hostile strategy is called a man-in-the-middle attack. Unless you think to check the originating IP address of your session on the server, you might never notice the deception.

The SSH known-host mechanism prevents such attacks. When an SSH client and server make a connection, each of them proves its identity to the other. Yes, not only does the server authenticate the client, as we saw earlier when the server checked Pat’s password, but the client also authenticates the server by public-key cryptography. In short, each SSH server has a secret, unique ID, called a host key, to identify itself to clients. The first time you connect to a remote host, a public counterpart of the host key gets copied and stored in your local account (assuming you responded “yes” to the client’s prompt about host keys, earlier). Each time you reconnect to that remote host, the SSH client checks the remote host’s identity using this public key.

Of course, it’s better to have recorded the server’s public host key before connecting to it the first time, since otherwise you are technically open to a man-in-the-middle attack that first time. Administrators can maintain systemwide known-hosts lists for given sets of hosts, but this doesn’t do much good for connecting to random new hosts around the world. Until a reliable, widely deployed method of verifying such keys securely exists (such as secure DNS, or X.509-based public-key infrastructure), this record-on-first-use mechanism is an acceptable compromise.

If authentication of the server fails, various things may happen depending on the reason for failure and the SSH configuration. Typically a warning appears on the screen, ranging from a repeat of the known-hosts message:

> Host key not found from the list of known hosts. Are you sure you want to continue connecting (yes/no)?

to more dire words:

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@ WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED! @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that the RSA host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
77:a5:69:81:9b:eb:40:76:7b:13:04:a9:6c:f4:9c:5d.
Please contact your system administrator.
Add correct host key in /home/smith/.ssh/known_hosts to get rid of this message.
Offending key in /home/smith/.ssh/known_hosts:36
```

If you answer yes, ssh allows the connection, but disables various features as a security precaution and doesn’t update your personal known-hosts database with the new key; you must do that yourself to make this message go away.

As the text of the message says, if you see this warning, you aren’t necessarily being hacked: for example, the remote host key may have legitimately changed for some reason. In some cases, even after reading this book, you won’t know the cause of these messages. If you need assistance, contact your system administrator or a knowledgeable friend, rather than take a chance and possibly compromise your password. We’ll cover these issues further when we discuss personal known hosts databases and how to alter the behavior of SSH clients with respect to host keys.

## The Escape Character

Let us return to the shell.isp.com example, just after you’d discovered the attachment in your remote email message and saved it to the remote file printme.pdf. In our original example, you then logged out of shell.isp.com and ran scp to transfer the file. But what if you don’t want to log out? If you’re using a workstation running a window system, you can open a new window and run scp. But if you’re using a lowly text terminal, or you’re not familiar with the window system running on your friend’s computer, there is an alternative. You can temporarily interrupt the SSH connection, transfer the file (and run any other local commands you desire), and then resume the connection.

ssh supports an escape character, a designated character that gets the attention of the SSH client. Normally, ssh sends every character you type to the server, but the escape character is caught by the client, alerting it that special commands may follow. By default, the escape character is the tilde (~), but you can change it. To reduce the chances of sending the escape character unintentionally, that character must be the first character on the command line, i.e., following a newline (Control-J) or return (Control-M) character. If not, the client treats it literally, not as an escape character. After the escape character gets the client’s attention, the next character entered determines the effect of the escape. For example, the escape character followed by a Control-Z suspends ssh like any other shell job, returning control to the local shell. Such a pair of characters is called an escape sequence. We cover these in detail in a later chapter.

To change the ssh escape character, use the –e command-line option. For example, type the following to make the percent sign (%) the escape character when connecting to shell.isp.com as user pat:

```bash
$ ssh -e "%" -l pat shell.isp.com
```

## Authentication by Cryptographic Key

In our running example, the user pat is authenticated by the SSH server via login password. Passwords, however, have serious drawbacks:

* In order for a password to be secure, it should be long and random, but such passwords are hard to memorize.
* A password sent across the network, even protected by an SSH secure channel, can be captured when it arrives on the remote host if that host has been compromised.
*  Most operating systems support only a single password per account. For shared accounts (e.g., a superuser account), this presents difficulties:
    * Password changes are inconvenient because the new password must be communicated to all people with access to the account.
    * Tracking usage of the account becomes difficult because the operating system doesn’t distinguish between the different users of the account.

To address these problems, SSH supports public-key authentication: instead of relying on the password scheme of the host operating system, SSH may use cryptographic keys. Keys are more secure than passwords in general and address all the weaknesses mentioned earlier.

## A Brief Introduction to Keys

A key is a digital identity. It’s a unique string of binary data that means “This is me, honestly, I swear.” And with a little cryptographic magic, your SSH client can prove to a server that its key is genuine, and you are really you.

An SSH identity uses a pair of keys, one private and one public. The private key is a closely guarded secret only you have. Your SSH clients use it to prove your identity to servers. The public key is, like the name says, public. You place it freely into your accounts on SSH server machines. During authentication, the SSH client and server have a little conversation about your private and public key. If they match (according to a cryptographic test), your identity is proven, and authentication succeeds.

The following sequence demonstrates the conversation between client and server. (It occurs behind the scenes, so you don’t need to memorize it or anything; we just thought you might be interested.)

1. Your client says, “Hey server, I’d like to connect by SSH to an account on your system, specifically, the account owned by user smith.”

2. The server says, “Well, maybe. First, I challenge you to prove your identity!” And the server sends some data, known as a challenge, to the client.

3. Your client says, “I accept your challenge. Here is proof of my identity. I made it myself by mathematically using your challenge and my private key.” This response to the server is called an authenticator.

4. The server says, “Thanks for the authenticator. I will now examine the smith account to see if you may enter.” Specifically, the server checks smith’s public keys to see if the authenticator “matches” any of them. (The “match” is another cryptographic operation.) If so, the server says, “OK, come on in!” Otherwise, the authentication fails.

Before you can use public-key authentication, some setup is required:

* You need a private key and a public key, known collectively as a key pair. You also need a secret passphrase to protect your private key.

* You need to install your public key on an SSH server machine.

## Generating Key Pairs with ssh-keygen

To use cryptographic authentication, you must first generate a key pair for yourself, consisting of a private key (your digital identity that sits on the client machine) and a public key (that sits on the server machine). To do this, use the ssh-keygen program to produce either a DSA or RSA key. The OpenSSH version of ssh-keygen requires you to specify the key type with the –t option (there is no default):

```bash
$ ssh-keygen -t dsa
Generating public/private dsa key pair.
Enter file in which to save the key (/home/dbarrett/.ssh/id_dsa): press ENTER
Enter passphrase (empty for no passphrase): ********
Enter same passphrase again: ********
Your identification has been saved in /home/pat/.ssh/id_dsa.
Your public key has been saved in /home/pat/.ssh/id_dsa.pub.
The key fingerprint is:
14:ba:06:98:a8:98:ad:27:b5:ce:55:85:ec:64:37:19 pat@shell.isp.com
```

On Tectia systems, ssh-keygen produces a DSA key by default, and also accepts the –t option:

```bash
$ ssh-keygen
Generating 2048-bit dsa key pair
1 ..oOo.oOo.oO
2 o.oOo.oOo.oO
3 o.oOo.oOo.oO
The program displays a “ripple” pattern to indicate progress; the characters are actually
overwritten on a single line
28 o.oOo.oOo.oO
Key generated.
2048-bit dsa, pat@shell.isp.com, Wed Jan 12 2005 20:22:21 -0500
Passphrase : **************
Again : **************
Private key saved to /home/pat/.ssh2/id_dsa_2048_a
Public key saved to /home/pat/.ssh2/id_dsa_2048_a.pub
```

Normally, ssh-keygen performs all necessary mathematics to generate a key, but on some operating systems you might be asked to assist it. Key generation requires some random numbers, and if your operating system doesn’t supply a random-number generator, you may be asked to type some random text or wiggle your mouse around. ssh-keygen uses the timings of your keystrokes to initialize its internal random-number generator. On a 3.2 GHz Pentium 4 system running Linux, a 1024- bit RSA key generates in less than one second; if your hardware is slower or heavily loaded, generation could take minutes. It can also take longer if the process runs out of random bits and ssh-keygen waits to collect more.

ssh-keygen then creates your local SSH directory (~/.ssh for OpenSSH or ~/.ssh2 for Tectia) if it doesn’t already exist, and stores the private and public components of the generated key in two files there. By default, their names are id_dsa and id_dsa.pub (OpenSSH) or id_dsa_2048_a and id_dsa_2048_a.pub (Tectia). SSH clients consider these to be your default identity for authentication purposes.

> Never reveal your private key and passphrase to anyone. They are just as sensitive as your login password. Anyone possessing them can impersonate you!

When created, the identity file is readable only by your account, and its contents are further protected by encrypting them with the passphrase you supplied during generation. We say “passphrase” instead of “password” both to differentiate it from a login password, and to stress that spaces and punctuation are allowed and encouraged. We recommend a passphrase at least 10–15 characters long and not a grammatical sentence.

ssh-keygen has numerous options for managing keys: changing the passphrase, choosing a different name for the key file, and so forth.

## Installing a Public Key on an SSH Server Machine

When passwords are used for authentication, the host operating system maintains the association between the username and the password. For cryptographic keys, you must set up a similar association manually. After creating the key pair on the local host, you must install your public key in your account on the remote host. A remote account may have many public keys installed for accessing it in various ways.

Returning to our running example, you must install a public key into the pat account on shell.isp.com. This is done by editing a file in the SSH configuration directory: ~/.ssh/ authorized_keys for OpenSSH or ~/.ssh2/authorization for Tectia.

## Public-Key Authentication

Public-key authentication is more secure than password authentication because:

![Figure-2-2](ScreenshotsForNotes/Chapter2/Figure_2_2.PNG)

* It requires two secret components—the identity file on disk, and the passphrase in your head—so both must be captured in order for an adversary to access your account. Password authentication requires only one component, the password, which might be easier to steal.
*  Neither the passphrase nor the key is sent to the remote host, just the authenticator discussed earlier. Therefore, no secret information is transmitted off the client machine.
*  Machine-generated cryptographic keys are infeasible to guess. Human-generated passwords are routinely cracked by a password-guessing technique called a dictionary attack. A dictionary attack may be mounted on the passphrase as well, but this requires stealing the private-key file first.

A host’s security can be greatly increased by disabling password authentication altogether and permitting only SSH connections by key.

## If You Change Your Key

Suppose you have generated a key pair, id_dsa and id_dsa.pub, and copied id_dsa.pub to a bunch of SSH server machines. All is well. Then one day, you decide to change your identity, so you run ssh-keygen a second time, overwriting id_dsa and id_dsa. pub. Guess what? Your previous public-key file is now invalid, and you must copy the new public key to all those SSH server machines again. This is a maintenance headache, so think carefully before changing (destroying!) a key pair. Some caveats:

* You are not limited to one key pair. You can generate as many as you like, stored in different files, and use them for diverse purposes.
* If you just want to change your passphrase, you don’t have to generate a new key pair. ssh-keygen has command-line options for replacing the passphrase of an existing key: –p for OpenSSH and –e for Tectia. In this case your public key remains valid since the private key hasn’t changed, just the passphrase for decrypting it.

## The SSH Agent

Each time you run ssh or scp with public-key authentication, you have to retype your passphrase. The first few times you might not mind, but eventually this retyping gets annoying. Wouldn’t it be nicer to identify yourself just once and have ssh and scp remember your identity until further notice (for example, until you log out), not prompting for your passphrase? In fact, this is just what an SSH agent does for you.

An agent is a program that keeps private keys in memory and provides authentication services to SSH clients. If you preload an agent with private keys at the beginning of a login session, your SSH clients won’t prompt for passphrases. Instead, they communicate with the agent as needed. The effects of the agent last until you terminate the agent, usually just before logging out. The agent program for both OpenSSH and Tectia is called ssh-agent.

Generally, you run a single ssh-agent in your local login session, before running any SSH clients. You can run the agent by hand, but people usually edit their login files (for example, ~/.login or ~/.xsession) to run the agent automatically. SSH clients communicate with the agent via a local socket or named pipe whose filename is stored in an environment variable, so all clients (and all other processes) within your login session have access to the agent. To try the agent, type:

```bash
$ ssh-agent $SHELL
```

where SHELL is the environment variable containing the name of your login shell. Alternatively, you could supply the name of any other shell, such as sh, bash, csh, tcsh, or ksh. The agent runs and then invokes the given shell as a child process. The visual effect is simply that another shell prompt appears, but this shell has access to the agent.

Once the agent is running, it’s time to load private keys into it using the ssh-add program. By default, ssh-add loads the key from your default identity file:

```bash
$ ssh-add
Enter passphrase for /home/you/.ssh/id_dsa: ********
Identity added: /home/you/.ssh/id_dsa (/home/you/.ssh/id_dsa)
```

Now ssh and scp can connect to remote hosts without prompting for your passphrase. Figure 2-3 shows the process.

ssh-add reads the passphrase from your terminal by default or, optionally, from standard input noninteractively. Otherwise, if you are running the X Window System with the DISPLAY environment variable set, and standard input isn’t a terminal, sshadd reads your passphrase using a graphical X program, ssh-askpass. This behavior is useful when calling ssh-add from X session setup scripts.

To force ssh-add to use X to read the passphrase, type ssh-add < /dev/ null at a command line.

![Figure-2-3](ScreenshotsForNotes/Chapter2/Figure_2_3.PNG)

ssh-add has further capabilities and can operate with multiple identity files. For now, here are a few useful commands. To load a key other than your default identity into the agent, provide the filename as an argument to ssh-add:

```bash
$ ssh-add my-other-key-file
```

You can also list the keys the agent currently holds:

```bash
$ ssh-add -l
```

delete a key from the agent in memory:

```bash
$ ssh-add -d name-of-key-file
```

or delete all keys from the agent in memory:

```bash
$ ssh-add -D
```

When running an SSH agent, don’t leave your terminal unattended while logged in. While your private keys are loaded in an agent, anyone may use your terminal to connect to any remote accounts accessible via those keys, without needing your passphrase! Even worse, a sophisticated intruder can extract your keys from the running agent and steal them.

If you use an agent, make sure to lock your terminal if you leave it while logged in. You can also use ssh-add -D to clear your loaded keys and reload them when you return. In addition, ssh-agent can be “locked” by ssh-add, to protect the agent from unauthorized users.

## Agents and Automation

Suppose you have a batch script that runs ssh to launch remote processes. If the script runs ssh many times, it prompts for your passphrase repeatedly, which is inconvenient for automation (not to mention annoying and error-prone). If you run an agent, however, your script can run without a single passphrase prompt.

## A More Complex Passphrase Problem

In our running example, we copied a file from the remote to the local host:

```bash
$ scp pat@shell.isp.com:printme.pdf imprime-moi.pdf
```

In fact, scp can copy a file from the remote host shell.isp.com directly to a third host running SSH on which you have an account named, say, “psmith”:

```bash
$ scp pat@shell.isp.com:printme.pdf psmith@other.host.net:imprime-moi.pdf
```

Rather than copying the file first to the local host and then back out again to the final destination, this command has shell.isp.com send it directly to other.host.net. However, if you try this, you run into the following problem:

```bash
$ scp pat@shell.isp.com:printme.pdf psmith@other.host.net:imprime-moi.pdf
Enter passphrase for RSA key 'Your Name <you@local.org>': ************
You have no controlling tty and no DISPLAY. Cannot read passphrase.
lost connection
```

What happened? When you run scp on your local machine, it contacts shell.isp.com and internally invokes a second scp command to do the copy. Unfortunately, the second scp command also needs the passphrase for your private key. Since there is no terminal session to prompt for the passphrase, the second scp fails, causing the original scp to fail. The SSH agent solves this problem: the second scp command simply queries your local SSH agent, so no passphrase prompting is needed.

The SSH agent also solves another, more subtle, problem in this example. Without the agent, the second scp (on shell.isp.com) needs access to your private-key file, but the file is on your local machine. So, you have to copy your private key file to shell. isp.com. This isn’t ideal; what if shell.isp.com isn’t a secure machine? Also, the solution doesn’t scale: if you have a dozen different accounts, it is a maintenance headache to keep your private key file on all of them. Fortunately, the SSH agent comes to the rescue once again. The remote scp process simply contacts your local SSH agent and authenticates, and the secure copy proceeds successfully, through a process called agent forwarding.

## Agent Forwarding

In the preceding example, the remote instance of scp has no direct access to your private key, since the agent is running on the local host, not the remote host. SSH provides agent forwarding to address this problem.

When agent forwarding is turned on,* the remote SSH server masquerades as a second ssh-agent, as shown in Figure 2-4. It takes authentication requests from your SSH client processes there, passes them back over the SSH connection to the local agent for handling, and relays the results back to the remote clients. In short, remote clients transparently get access to the local ssh-agent. Since any programs executed via ssh on the remote side are children of the server, they all have access to the local agent just as if they were running on the local host.

![Figure-2-4](ScreenshotsForNotes/Chapter2/Figure_2_4.PNG)

In our double-remote scp example, here is what happens when agent forwarding comes into play (see Figure 2-5):

1. You run the command on your local machine: ```$ scp pat@shell.isp.com:printme.pdf psmith@other.host.net:imprime-moi.pdf```

2. This scp process contacts your local agent and authenticates you to shell.isp.com.

3. A second scp command is automatically launched on shell.isp.com to carry out the copy to other.host.net.

4. Since agent forwarding is turned on, the SSH server on shell.isp.com poses as an agent.

5. The second scp process tries to authenticate you to other.host.net by contacting the “agent” that is really the SSH server on shell.isp.com.

6. Behind the scenes, the SSH server on shell.isp.com communicates with your local agent, which constructs an authenticator proving your identity and passes it back to the server.

7. The server verifies your identity to the second scp process, and authentication succeeds on other.host.net.

8. The file copying occurs.

![Figure-2-5](ScreenshotsForNotes/Chapter2/Figure_2_5.PNG)

Agent forwarding works over multiple connections in a series, allowing you to ssh from one machine to another, and then to another, with the agent connection following along the whole way. These machines may be progressively less secure, but agent forwarding doesn’t send your private key to the remote host: it just relays authentication requests back to the first host for processing. Therefore, you don’t have to copy your private key to other machines.

## Connecting Without a Password or Passphrase

One of the most frequently asked questions about SSH is: “How can I connect to a remote machine without having to type a password or passphrase?” As you’ve seen, an SSH agent can make this possible, but there are other methods as well, each with different trade-offs. Here we list the available methods with pointers to the sections discussing each one.

To use SSH clients for interactive sessions without a password or passphrase, you have several options:

* Public-key authentication with an agent

* Hostbased authentication

* Kerberos authentication

Another way to achieve passwordless logins is to use an unencrypted private key with no passphrase. Although this technique can be appropriate for automation purposes, never do this for interactive use. Instead, use the SSH agent, which provides the same benefits with much greater security. Don’t use unencrypted keys for interactive SSH!

On the other hand, noninteractive, unattended programs such as cron jobs or batch scripts may also benefit from not having a password or passphrase. In this case, the different techniques raise some complex issues, and we discuss their relative merits and security issues later