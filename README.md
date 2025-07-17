Perfect — let’s go step by step:

---

## ✅ Part 1: **Understanding `cp` and `/mnt` in WSL**

### 📌 What is `/mnt`?

In WSL, **your Windows drives (C:, D:, etc.) are mounted under `/mnt/`**:

| Windows Drive            | WSL Path                     |
| ------------------------ | ---------------------------- |
| `C:\`                    | `/mnt/c/`                    |
| `D:\`                    | `/mnt/d/`                    |
| `C:\Users\dell\Pictures` | `/mnt/c/Users/dell/Pictures` |

**cd /mnt/c= we will go to folder name c**


### 🧱 Example: Copy a file using `cp`

```bash
cp /mnt/c/Users/dell/Pictures/jailal.pem ~/.ssh/
```

This means:

* Copy `jailal.pem` from **Windows Pictures folder**
* To your **Linux home `.ssh` folder**

You can confirm it with:

```bash
ls ~/.ssh
```

---

## ✅ Part 2: **Create an SSH Alias (`ssh ec2`)**

You can configure an alias so you **don’t have to type the full `ssh -i` command** every time.

### 🔧 Step-by-step:

#### 1. **Create SSH config file (if it doesn't exist)**

```bash
nano ~/.ssh/config
```

Paste this into the file:

```ssh
Host ec2
    HostName 52.87.213.222
    User ubuntu
    IdentityFile ~/.ssh/jailal.pem
```

#### 2. **Save and exit** (in nano: press `Ctrl + O`, then `Enter`, then `Ctrl + X`)

#### 3. **Fix permissions on the config file** (important!)

```bash
chmod 600 ~/.ssh/config
```

---

### ✅ Now Just Use:

```bash
ssh ec2
```

🎉 That’s it — no need to type `ssh -i` or remember the IP!

---

## 💡 Bonus: If EC2 IP Changes Often (Elastic IP not used)

If you don't use Elastic IP and the EC2 public IP changes, just **edit your config**:

```bash
nano ~/.ssh/config
```

Update the `HostName` field with the new IP.

ec2 ssh -i .pemubuntu@ip

Great question! Let's break it down clearly:

---

## 💡 What is `$USER` in Linux/WSL?

`$USER` is a **shell variable** that automatically holds the **username of the currently logged-in user**.

### 🔎 Example:

If you're logged in as `jailal`, then:

```bash
echo $USER
```

will output:

```
jailal
```

---

## ✅ Why use `sudo chown $USER: ...`?

```bash
sudo chown $USER: ~/.ssh/jailal.pem
```

is the same as:

```bash
sudo chown jailal: ~/.ssh/jailal.pem
```

So `$USER` lets you **write scripts or commands** that will work for **any user**, without hardcoding the username.

### 🔄 It's dynamic and reusable!

* **Good for automation**
* **Cleaner and more portable**

---

## 📌 Summary

| Term    | Meaning                    |
| ------- | -------------------------- |
| `$USER` | The current Linux/WSL user |
| `chown` | Change file owner          |
| `chmod` | Change file permissions    |

---

Excellent — let’s now clearly explain **how numbers are used in `chmod`** along with how they relate to **file permissions** in Linux.

---

## 🔢 chmod with Numbers (Octal Notation)

Linux permissions for each file are based on **three sets** of permissions:

```
[ user ][ group ][ others ]
```

Each permission (read/write/execute) is represented by a number:

| Permission | Symbol | Value |
| ---------- | ------ | ----- |
| Read       | `r`    | 4     |
| Write      | `w`    | 2     |
| Execute    | `x`    | 1     |

To get the numeric value for each set (user, group, others), you **add the values** of the permissions you want.

---

### 🔓 Examples:

| chmod Value | Meaning | Explanation                 |
| ----------- | ------- | --------------------------- |
| `7`         | `rwx`   | 4 + 2 + 1 = 7 (full access) |
| `6`         | `rw-`   | 4 + 2 = 6                   |
| `5`         | `r-x`   | 4 + 1 = 5                   |
| `4`         | `r--`   | read-only                   |
| `0`         | `---`   | no permissions              |

---

### 📘 Common `chmod` Number Combinations

```bash
chmod 755 file.sh
```

Means:

| Who    | Value | Permissions |
| ------ | ----- | ----------- |
| User   | 7     | `rwx`       |
| Group  | 5     | `r-x`       |
| Others | 5     | `r-x`       |

---

```bash
chmod 600 secret.txt
```

Means:

| Who    | Value | Permissions |
| ------ | ----- | ----------- |
| User   | 6     | `rw-`       |
| Group  | 0     | `---`       |
| Others | 0     | `---`       |

✅ Used for sensitive files like SSH keys (`chmod 400` or `600`).

---

### 🧠 Summary Table:

| Command     | Effect                                |
| ----------- | ------------------------------------- |
| `chmod 777` | Everyone can read/write/execute       |
| `chmod 755` | User full, others can read/execute    |
| `chmod 700` | Only user can read/write/execute      |
| `chmod 644` | User can read/write, others read-only |
| `chmod 600` | User read/write, others no access     |
| `chmod 400` | User read-only (for `.pem` files)     |

---
