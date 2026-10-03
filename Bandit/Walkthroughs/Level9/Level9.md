## Bandit Level 9 - 10

> **Level Goal:** The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

Commands you may need to solve this level

~~~ bash
grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd
~~~

---

## Connection Details

| Field    | Value                                                                        |
| -------- | ---------------------------------------------------------------------------- |
| Host     | `bandit.labs.overthewire.org`                                                |
| Port     | `2220`                                                                       |
| Username | `bandit9`                                                                  |
| Password | *found in [Bandit Level 8 → Level 9](../Level8/Level8.md)* |

---

## Commands Used

- `ssh` — Connects securely to the remote machine
- `[comando]` — [Descripción]
- `[comando]` — [Descripción]
- `[comando]` — [Descripción]
- `exit` — Close the SSH session

---

## Solution

### Step 1 — Connect to the Server

If you haven't used SSH before, see [How to connect to the server](../../sshConnection_guide) for a full explanation.

~~~ bash
ssh bandit9@bandit.labs.overthewire.org -p 2220
~~~

Use the password you found at the end of [Bandit Level 8 → Level 9](../Level8/Level8.md).

---

### Step 2 — [Título del paso]

~~~ bash
bandit9@bandit:~$ [comando]
[salida]
~~~

[Explicación breve de lo que se observa]

~~~ bash
bandit9@bandit:~$ [comando]
[salida]
~~~

[Explicación breve de lo que se observa]

---

### Step 3 — Solution

~~~ bash
bandit9@bandit:~$ [comando final]
<password_here>
~~~

#### Explanation

[Explicación de por qué se usó este comando y cómo funciona]

[Explicación de qué pasaría con un enfoque alternativo / errores comunes]

~~~ bash
bandit9@bandit:~$ [comando alternativo]
~~~

[Explicación del resultado y por qué no sirve / por qué sí]

---

#### Screenshots/

<img src = "../../Assets/LVL9/image1.png">
<img src = "../../Assets/LVL9/image2.png">

---

## Key Takeaways

- [Aprendizaje clave 1]
- [Aprendizaje clave 2]
- [Aprendizaje clave 3]
- [Aprendizaje clave 4]