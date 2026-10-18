## Bandit Level 11 - 12

> **Level Goal:** The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

Commands you may need to solve this level

~~~ bash
grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd
~~~

---

## Connection Details

| Field    | Value                                                          |
| -------- | -------------------------------------------------------------- |
| Host     | `bandit.labs.overthewire.org`                                  |
| Port     | `2220`                                                         |
| Username | `bandit11`                                                      |
| Password | *found in [Bandit Level 10 → Level 11](../Level10/Level10.md)*     |

---

## Commands Used

- `ssh` — Connects securely to the remote machine
- `exit` — Closes the SSH session
- `ls` `ls -a` — Shows the directory content including hidden files/directories
- `cat` — Reads a file and writes it in the terminal
- `man` — An interface to the system reference manuals
- `tr` — Swaps characters for others, or deletes them

---

## Solution

### Step 1 — Connect to the Server

If you haven't used SSH before, see [How to connect to the server](../../sshConnection_guide) for a full explanation.

~~~ bash
ssh banditX@bandit.labs.overthewire.org -p 2220
~~~

Use the password you found at the end of [Bandit Level W → Level X](../LevelW/LevelW.md).

---

### Step 2 — Enumeration
# list the home directory
list the contents of the directory with `ls -a`, we'll find the target file.

~~~ bash
bandit11@bandit:~$ ls -a
.  ..  .bash_logout  .bashrc  .profile  data.txt
bandit11@bandit:~$ file data.txt && wc -l data.txt
data.txt: ASCII text
1 data.txt
bandit11@bandit:~$ cat data.txt
Gur cnffjbeq vf TEBbmJCB8DlA0zTewHxVQ0JPLxMvDkeA
~~~

The target is `data.txt`, and it's encrypted with a "Caesar Cipher", a simple substitution cipher used to send messages encrypted by character substitution in an alphabet (26 characters in English), using other letters in their position instead. For example, ROT13 is a simple Caesar Cipher with a +13 rotation, which means that the alphabet in which we're going to send the message starts with `N` instead of `A`:

Each letter in **SET1** is replaced by the letter in the same position in **SET2**:

| **SET1** | A | B | C | D | E | F | G | H | I | J | K | L | M |
|:--|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| | ↓ | ↓ | ↓ | ↓ | ↓ | ↓ | ↓ | ↓ | ↓ | ↓ | ↓ | ↓ | ↓ |
| **SET2** | N | O | P | Q | R | S | T | U | V | W | X | Y | Z |

| **SET1** | N | O | P | Q | R | S | T | U | V | W | X | Y | Z |
|:--|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| | ↓ | ↓ | ↓ | ↓ | ↓ | ↓ | ↓ | ↓ | ↓ | ↓ | ↓ | ↓ | ↓ |
| **SET2** | A | B | C | D | E | F | G | H | I | J | K | L | M |

If we want to send ***HELLO*** in Caesar Cipher, we're gonna send ***URYYB*** instead.

In Bash, we can encrypt/decrypt Caesar Cipher using the tool `tr`. In the command's man page it says:

`Usage: tr [OPTION]... SET1 [SET2]`

Every "SET" is an alphabet, a 26-char English alphabet like the ones right above. `tr` encrypts/decrypts using a base alphabet [SET1] (standard A-Z for encryption), and an equivalent alphabet in which the message is encrypted [SET2]. To encrypt "HELLO" in ROT13, we can write it like this instead of writing both 26-char alphabets:

- `echo HELLO | tr A-Z N-ZA-M`

`-` It says "From A to Z", a shorthand instead of writing the entire alphabet. `N-ZA-M` is a shorthand for writing the **SET2**.

The `tr` command just accepts ordered character sequences, always going forward and never an alphabet that goes back like `N-M`, because `M` is behind `N`. Instead, we're using `N-ZA-M`, which combines 2 ranges to get one 26-char set:
- `N-Z` - A range from N (where +13 starts) to Z
- `A-M` - A range from A (after Z, it starts again to complete 26 chars) to M (final char in the 26-char alphabet that starts at N)

If we also include lowercase, the command would be something like this in ROT13:
- `echo "Hello Friend" | tr A-Za-z N-ZA-Mn-za-m`
Output: `Uryyb Sevraq`, literally the same thing.

---

### Step 3 — Getting the password

Now that we understand how to decrypt the Caesar Cipher, we can decrypt the message knowing that _"... all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions"_

~~~ bash
bandit11@bandit:~$ ls -a
.  ..  .bash_logout  .bashrc  .profile  data.txt
bandit11@bandit:~$ cat data.txt | tr A-Za-z N-ZA-Mn-za-m
The password is <password_here>
~~~

---

#### Screenshots

<img src = ../../Assets/LVL11/image1.png>
<img src = ../../Assets/LVL11/image2.png>

---

## Key Takeaways

- Caesar Cipher is a simple substitution cipher that replaces letters by their position in the alphabet
- Caesar Cipher isn't really secure, it can be easily broken by a brute-force attack (trying all the possible shifts) and with an entropy study
- We can use `tr` to encrypt/decrypt in Caesar Cipher with `tr SET1 SET2` and also erase patterns from a string with `tr -d [PATTERN]`