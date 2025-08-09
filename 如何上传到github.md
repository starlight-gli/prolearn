## 一、直接上传

我帮你用最简洁的方式总结一下**上传到 GitHub 远程仓库**的步骤（假设你已经在 GitHub 创建了一个空仓库，并且本地已经写好代码/文件）：

------

### **1. 进入你的本地项目目录**

```bash

cd 你的项目目录路径
```

------

### **2. 初始化 Git 仓库（如果还没初始化）**

```bash

git init
```

------

### **3. 绑定远程仓库地址（第一次需要做）**

**SSH 方式**（推荐）：

```bash

git remote add origin git@github.com:用户名/仓库名.git
```

**HTTPS 方式**：

```bash

git remote add origin https://github.com/用户名/仓库名.git
```

------

### **4. 添加文件到暂存区**

```bash

git add .
```

（`.` 表示添加所有改动的文件）

------

### **5. 提交到本地仓库**

```bash

git commit -m "提交说明"
```

------

### **6. 推送到 GitHub 远程仓库**

```bash

git push -u origin main
```

> 如果远程仓库默认分支是 `master`，就把 `main` 改成 `master`。

------

**总结口诀**：

```bash

cd 目录 → git init → git remote add → git add . → git commit -m "" → git push
```

## 二、`git clone`下载本地仓库上传

### 用 `git clone` 之后再上传的步骤

1. **先从远程仓库克隆**（这样本地就和远程已经关联好了）

   ```bash
   
   git clone git@github.com:用户名/仓库名.git
   ```

2. **进入项目目录**

   ```bash
   
   cd 仓库名
   ```

3. **添加或修改文件**

   ```bash
   
   echo "Hello" > test.txt
   ```

4. **添加到暂存区**

   ```bash
   
   git add test.txt
   ```

5. **提交到本地仓库**

   ```bash
   
   git commit -m "添加 test.txt"
   ```

6. **推送到远程仓库**（这一步才是上传）

   ```bash
   
   git push
   ```

## 三、一些问题

### 方案一：先拉取远程最新内容，合并到本地，再推送

执行：

```bash

git pull origin main --rebase
```

这条命令的作用是：

- 把远程 `main` 分支最新的提交取下来，
- 然后把你本地的提交**放到它们后面（rebase）**，
- 保持历史整洁。

成功后再推送：

```bash

git push origin main
```

### 方案二：如果你确定要强制推送（危险，可能覆盖远程提交）

```bash

git push -f origin main
```

> **警告：** 这个命令会强制覆盖远程仓库，可能会丢失远程别人已经提交的内容，慎用！