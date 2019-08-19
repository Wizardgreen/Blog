---
title: Docker
tags: 
  - Docker
  - Note
---

> <center>Docker 指令備忘錄</center>

### Image 類
images - 列出所有 Image
rmi [image name / id] - 移除 Imgae
  -f 強制移除 (可能被 Container 或 Image 需要)

### Container 類
ps - 列出所有啟用的 Container
  -a - 列出所有 Container
rm [container name / id] - 移除 Container
  -f 強制移除 (可能被 Container 或 Image 需要)