## Bandit Level 10 - 11

> **Level Goal:** The password for the next level is stored in the file data.txt, which contains base64 encoded data

Commands you may need to solve this level

~~~ bash
ls , cd , cat , file , du , find
~~~

---

## Connection Details

| Field    | Value                                                              |
| -------- | ------------------------------------------------------------------ |
| Host     | `bandit.labs.overthewire.org`                                      |
| Port     | `2220`                                                              |
| Username | `bandit10`                                                         |
| Password | *found in [Bandit Level 9 → Level 10](../Level9/Level9.md)*     |

---

## Commands Used

- `ssh` — Connects securely to the remote machine
- `exit` — Close the SSH session
- `ls` `la -a` — Shows the directory content including hidden files/directories
- `file` — returns information about the type of file we're working with. ASCII text, data (binary), etc
- `wc -l` — counts lines in a file
- `cat` — read a file and writes it in the terminal
- `man` — an interface to the system reference manuals
---

## Solution

### Step 1 — Connect to the Server

If you haven't used SSH before, see [How to connect to the server](../../sshConnection_guide) for a full explanation.

~~~ bash
ssh banditn1@bandit.labs.overthewire.org -p 2220
~~~

Use the password you found at the end of [Bandit Level 9 → Level 10](../Level9/Level9.md).

---

## Step 2 — Enumeration
# list the home directory
list the contents of the directory with `ls -a`, we'll find the target file (data.txt). To enumerate, we'll use `file` and `wc -l` (`-l` is the parameter to count lines) to get more information about the filetype in lines count, running `wc -l` right after `file` with '&&' pipe.

~~~ bash
bandit10@bandit:~$ ls -a
.  ..  .bash_logout  .bashrc  .profile  data.txt
bandit10@bandit:~$ file data.txt && wc -l data.txt
data.txt: ASCII text
1 data.txt
bandit10@bandit:~$ cat data.txt
VGhlIHBhc3N3b3JkIGlzIHBZZk9ZNkh3VXNEajVyTDlVdnloVTdNQ212OHZONVJvCg==
bandit10@bandit:~$ #so, this is base64 encode data
bandit10@bandit:~$ man base64
~~~

The target (`data.txt`) contains a text line encoded in base64.
base64 is a codification that allows binaries to travel through the network safely without being corrupted or changed in the process: encodes bits into ASCII text, using numbers 0-9, lowercase/uppercase english letters, and '+' '/' characters. Base64 isn't an encryption. That's the reason why it doesn't require a key to get its content and at the same time, it isn't 'secure' in terms of cybersecurity. 'Securing' credentials, API keys and `.env` files just with base64 encoding, is a bad practice in the industry that some folks still do nowadays. And for hackers, is a vulnerability that can be exploited and we should always consider. 

 If you wanna know more about it, check out the [Base64 guide](base64Encoding.md) that I've made.


---

### Step 3 — Getting the password

~~~ bash
bandit10@bandit:~$ base64 --help
...
...
Usage: base64 [OPTION]... [FILE]

Arguments:
  [file]...  

Options:
  -d, --decode          decode data [aliases: -D]
~~~

We see that we can decode a base64 encoded string in a file using `base64 -d [FILE]`, so let's use it to get the password:

~~~ bash
bandit10@bandit:~$ base64 -d data.txt
The password is <password_here>
~~~

---

#### Screenshots

<img src = ../../Assets/LVL10/image1.png>
<img src = ../../Assets/LVL10/image2.png>

---

## Key Takeaways

- Base64 is codification used for allowing binaries, JSON strings, etc to travel saffely through the network without being corrupted
- Base64 isn't an encryption, but a codification. We can decode any base64 encoded file/string with `base64 -d [FILE]` and `echo [string] | base64 -d`.
- Use real encryption (AES-256-GCM) and a secrets manager, never just Base64 to store API keys, credentials, etc.