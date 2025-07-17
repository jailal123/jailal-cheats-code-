

### 🔧 **Issue: Jenkins Cannot Access Docker Daemon**

**Error:**

```bash
permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock
```

**Cause:**
Jenkins was running as the `jenkins` user, which did not have permission to access the Docker socket.

---

### ✅ **Resolution Steps**

1. **Add Jenkins to the Docker Group:**

```bash
sudo usermod -aG docker jenkins
```

2. **Restart Jenkins (to apply group changes):**

```bash
sudo systemctl restart jenkins
```

3. *(Optional)* Restart Docker (if needed):

```bash
sudo systemctl restart docker
```

4. **Verify Access:**

```bash
sudo -u jenkins docker ps
```

✅ After these steps, Jenkins successfully accessed Docker, and the pipeline ran without Docker permission errors.

---

### 🐳 **Docker Permission Denied Debug Summary (`docker ps`)**

#### ❌ **Error:**  local machine ❌❌❌❌❌❌❌ LOCAL MACHINE ❌❌❌❌❌❌

```
permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock for local machine 
```

---

### ✅ **Step-by-Step Fix**

#### 1. **Check socket permissions**

```bash
ls -l /var/run/docker.sock
```

Should show:

```
srw-rw---- 1 root docker ...
```

> ✅ If it does: Good — continue.

---

#### 2. **Ensure your user is in the `docker` group**

```bash
groups
```

If `docker` is missing:

```bash
sudo usermod -aG docker $USER
```

---

#### 3. **Apply group membership**

**Option A (recommended):**

* Log out and log back in to apply group changes

**Option B (quick fix for current session):**

```bash
newgrp docker
```

---

#### 4. **Test**

```bash
docker ps
```

✅ If no error → you're done!

---

### 🛠 Optional: Restart Docker daemon (if needed)

```bash
sudo systemctl restart docker
```

---

### 💡 Temporary workaround:

If you still face issues:

```bash
sudo docker ps
```

> But avoid `sudo` for long-term use — prefer fixing group access.


