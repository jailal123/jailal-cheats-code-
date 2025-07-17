Here’s the same Jenkins pipeline breakdown using **high-contrast ANSI-style colors** (perfect for terminal-style presentations or docs that stand out visually):

---

### 🎯 **Jenkins Pipeline Execution Flow with Docker**

🖥️ **High-Contrast Code Explanation**

```ansi
[1;32m# ✅ STEP 1: Triggered by User[0m
Started by user jailal

[1;34m# 📦 STEP 2: Git Checkout[0m
Cloning from: https://github.com/iam-veeramalla/Jenkins-Zero-To-Hero
Workspace: /var/lib/jenkins/workspace/jailal

[1;34m# 🔁 Git Operations[0m
> git fetch --all
> git checkout 3028953   [2m# (specific commit)[0m

[1;33m# 🐳 STEP 3: Check Docker Image Locally[0m
> docker inspect -f . node:16-alpine
[1;31mError: No such object: node:16-alpine[0m

[1;33m# 📥 STEP 4: Pull Docker Image[0m
> docker pull node:16-alpine
✔ Downloaded layers
✔ Image available: node:16-alpine (v16.20.2)

[1;36m# 🚀 STEP 5: Run Container with Workspace Mounted[0m
docker run -t -d \
  -u 111:113 \
  -w /var/lib/jenkins/workspace/jailal \
  -v /var/lib/jenkins/workspace/jailal:/var/lib/jenkins/workspace/jailal \
  -v /var/lib/jenkins/workspace/jailal@tmp:/var/lib/jenkins/workspace/jailal@tmp \
  node:16-alpine cat

[1;35m# 🧪 STEP 6: Test Inside Container[0m
> node --version
[1;32mv16.20.2[0m

[1;31m# 🧹 STEP 7: Clean Up[0m
> docker stop <container_id>
> docker rm -f --volumes <container_id>

[1;32m# 🎉 STEP 8: Pipeline Success[0m
Finished: SUCCESS
```

---

### 📊 **Summary Table**

| 🔢 Step      | 🧾 Description                        | 🔍 Output            |
| ------------ | ------------------------------------- | -------------------- |
| ✅ Trigger    | Jenkins pipeline triggered manually   | jailal               |
| 📦 Git Clone | Code pulled from GitHub repo          | Jenkins-Zero-To-Hero |
| 🐳 Docker    | Pulled `node:16-alpine` image         | Docker Hub           |
| 🧪 Test      | Ran `node --version` inside container | `v16.20.2`           |
| 🧹 Cleanup   | Container stopped and removed         | ✅                    |
| 🎯 Status    | Pipeline completed successfully       | `Finished: SUCCESS`  |

---

Would you like this in HTML, PDF, or slide format for better presentation?
