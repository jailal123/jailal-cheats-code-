Sure! Here's a **short note** summarizing the issue and how it was resolved:

---

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

Let me know if you'd like this formatted for a report or documentation!
