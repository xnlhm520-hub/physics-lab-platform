# Git使用指南（小白版）

> **版本**：v1.0  
> **最后更新**：2024-01-01  
> **适用对象**：Git初学者，需要理解操作原理的开发者

## 📚 目录

- [一、Git是什么，为什么需要它](#一git是什么为什么需要它)
- [二、Git的核心概念（理解这些很重要）](#二git的核心概念理解这些很重要)
- [三、Git的安装和配置](#三git的安装和配置)
- [四、日常开发流程（最常用）](#四日常开发流程最常用)
- [五、分支管理（团队协作必备）](#五分支管理团队协作必备)
- [六、远程仓库操作](#六远程仓库操作)
- [七、常见问题处理](#七常见问题处理)
- [八、最佳实践](#八最佳实践)

---

## 一、Git是什么，为什么需要它

### 1.1 Git是什么？

**简单理解**：Git是一个**版本控制系统**，就像给你的代码拍"快照"，记录每次修改的内容。

**为什么叫"版本控制"？**
- 想象你在写一篇文章，每次修改后都保存一个副本
- 如果改错了，可以回到之前的版本
- 可以查看每次修改了什么内容
- 多人协作时不会互相覆盖代码

### 1.2 为什么需要Git？

#### 场景1：代码丢失问题

**没有Git的情况**：
```
周一：写了用户登录功能（文件：login.js）
周二：修改了登录功能，保存了login.js（覆盖了周一的代码）
周三：发现周二改错了，想回到周一的版本
结果：周一的代码已经丢失了！😱
```

**有Git的情况**：
```
周一：写了用户登录功能，用Git保存（commit）
周二：修改了登录功能，用Git保存（commit）
周三：发现周二改错了，用Git回到周一的版本
结果：轻松恢复！✅
```

#### 场景2：多人协作问题

**没有Git的情况**：
```
开发者A：修改了商品列表功能，保存到服务器
开发者B：也修改了商品列表功能，保存到服务器
结果：B的代码覆盖了A的代码，A的工作丢失了！😱
```

**有Git的情况**：
```
开发者A：修改了商品列表功能，提交到Git
开发者B：也修改了商品列表功能，提交到Git
Git自动合并：保留两个人的修改，如果有冲突会提示
结果：两个人的工作都保留了！✅
```

#### 场景3：代码审查问题

**没有Git的情况**：
```
开发者：我昨天改了什么？不记得了...
项目经理：这个bug是什么时候引入的？不知道...
```

**有Git的情况**：
```
开发者：git log 查看所有提交记录
项目经理：git blame 查看每行代码是谁改的
结果：所有修改都有记录，可追溯！✅
```

### 1.3 Git vs 其他版本控制工具

| 工具 | 特点 | 适用场景 |
|------|------|---------|
| **Git** | 分布式、速度快、功能强大 | 现代软件开发（推荐） |
| SVN | 集中式、简单 | 老项目 |
| 手动备份 | 复制文件夹 | 不推荐，容易出错 |

**为什么选择Git？**
- ✅ 分布式：每个开发者都有完整的代码历史
- ✅ 速度快：操作本地仓库，不需要网络
- ✅ 功能强大：分支、合并、回退等操作灵活
- ✅ 行业标准：GitHub、GitLab等平台都使用Git

---

## 二、Git的核心概念（理解这些很重要）

### 2.1 三个区域：工作区、暂存区、仓库

这是Git最重要的概念！理解了这三个区域，Git操作就清晰了。

#### 工作区（Working Directory）

**是什么**：你正在编辑的文件夹，就是你看到的代码文件。

**为什么需要**：这是你写代码的地方，就像你的"工作台"。

**示例**：
```
你的项目文件夹：
├── src/
│   ├── login.js      ← 你正在编辑这个文件
│   └── product.js    ← 你正在编辑这个文件
└── README.md
```

#### 暂存区（Staging Area / Index）

**是什么**：一个"准备区"，告诉Git哪些文件要保存。

**为什么需要**：不是所有修改都要保存，你可以选择性地保存某些文件。

**类比**：
- 工作区 = 你的购物车（所有商品）
- 暂存区 = 收银台（你决定要买的商品）
- 仓库 = 你的家（已经买回家的商品）

**示例**：
```
你修改了3个文件：
- login.js      ← 改好了，要保存
- product.js   ← 改好了，要保存
- test.js       ← 还在测试，不保存

操作：
git add login.js product.js  ← 把这两个文件放到暂存区
（test.js 还在工作区，不会被保存）
```

#### 仓库（Repository）

**是什么**：Git保存代码历史的地方，包含所有版本的快照。

**为什么需要**：这是代码的"档案馆"，记录每次修改。

**结构**：
```
仓库（.git文件夹，隐藏的）
├── 版本1：初始代码
├── 版本2：添加了登录功能
├── 版本3：添加了商品列表
└── 版本4：修复了bug
```

### 2.2 提交（Commit）

**是什么**：把暂存区的文件保存到仓库，创建一个新的版本快照。

**为什么需要**：这是"保存"操作，记录你完成了什么工作。

**提交包含什么**：
- 修改的文件内容
- 提交信息（你写的说明）
- 提交时间
- 提交者信息

**示例**：
```bash
git commit -m "添加用户登录功能"
```

**提交信息的重要性**：
- ✅ 好的提交信息：`"修复商品列表分页bug"`
- ❌ 不好的提交信息：`"修改"` 或 `"更新"`

### 2.3 分支（Branch）

**是什么**：代码的"平行宇宙"，可以在不影响主代码的情况下开发新功能。

**为什么需要**：
- 主分支（main/master）保持稳定
- 新功能在独立分支开发
- 开发完成后再合并到主分支

**类比**：
```
主分支（main）= 正式版软件
功能分支（feature）= 实验版软件
修复分支（fix）= 补丁版软件
```

**示例**：
```
main分支：稳定的代码
  │
  ├─ feature/login分支：开发登录功能
  │
  └─ feature/product分支：开发商品功能
```

### 2.4 远程仓库（Remote Repository）

**是什么**：放在服务器上的Git仓库，团队成员共享代码的地方。

**为什么需要**：
- 本地仓库 = 你的电脑（只有你能访问）
- 远程仓库 = 服务器（所有人都能访问）

**常见远程仓库**：
- GitHub：全球最大的代码托管平台
- GitLab：企业常用
- Gitee：国内常用

**工作流程**：
```
1. 从远程仓库拉取代码（git pull）
2. 在本地修改代码
3. 提交到本地仓库（git commit）
4. 推送到远程仓库（git push）
```

---

## 三、Git的安装和配置

### 3.1 安装Git

#### Windows系统

**步骤1：下载Git**
- 访问：https://git-scm.com/download/win
- 下载最新版本

**步骤2：安装**
- 双击安装包
- 一路"下一步"，使用默认设置即可

**步骤3：验证安装**
```bash
# 打开命令提示符（CMD）或PowerShell
git --version
# 应该显示：git version 2.x.x
```

#### Mac系统

**方法1：使用Homebrew（推荐）**
```bash
brew install git
```

**方法2：下载安装包**
- 访问：https://git-scm.com/download/mac

#### Linux系统

```bash
# Ubuntu/Debian
sudo apt-get install git

# CentOS/RHEL
sudo yum install git
```

### 3.2 配置Git（第一次使用必须配置）

**为什么需要配置**：Git需要知道你是谁，这样提交记录才会显示你的名字。

#### 配置用户名和邮箱

```bash
# 设置你的名字（会显示在提交记录中）
git config --global user.name "你的名字"

# 设置你的邮箱（会显示在提交记录中）
git config --global user.email "your.email@example.com"
```

**示例**：
```bash
git config --global user.name "张三"
git config --global user.email "zhangsan@example.com"
```

**为什么用--global**：
- `--global`：全局配置，所有项目都用这个配置
- 不加`--global`：只对当前项目生效

#### 查看配置

```bash
# 查看所有配置
git config --list

# 查看特定配置
git config user.name
git config user.email
```

#### 其他常用配置

```bash
# 设置默认编辑器（提交时写说明用的）
git config --global core.editor "code --wait"  # VS Code
# 或
git config --global core.editor "vim"          # Vim

# 设置默认分支名（新版本Git默认是main）
git config --global init.defaultBranch main

# 设置换行符处理（Windows推荐）
git config --global core.autocrlf true
```

---

## 四、日常开发流程（最常用）

### 4.1 初始化仓库（第一次使用）

#### 场景1：创建新项目

**步骤**：
```bash
# 1. 创建项目文件夹
mkdir my-project
cd my-project

# 2. 初始化Git仓库
git init

# 3. 创建第一个文件
echo "# 我的项目" > README.md

# 4. 添加到暂存区
git add README.md

# 5. 提交到仓库
git commit -m "初始化项目"
```

**为什么这样操作**：
- `git init`：在当前文件夹创建`.git`文件夹（隐藏的），这是Git仓库
- `git add`：告诉Git要保存哪些文件
- `git commit`：真正保存文件到仓库

#### 场景2：克隆已有项目

**步骤**：
```bash
# 从远程仓库克隆项目
git clone https://github.com/username/repository.git

# 进入项目文件夹
cd repository
```

**为什么用clone**：
- 自动下载所有代码
- 自动设置远程仓库地址
- 自动创建`.git`文件夹

### 4.2 日常开发工作流

这是你每天都会用的流程！

#### 完整流程

```bash
# 1. 查看当前状态（看看改了什么）
git status

# 2. 查看具体修改内容（可选）
git diff

# 3. 添加要保存的文件到暂存区
git add 文件名
# 或添加所有修改的文件
git add .

# 4. 提交到本地仓库
git commit -m "描述你做了什么"

# 5. 推送到远程仓库（如果是在团队中）
git push
```

#### 详细说明每个步骤

**步骤1：git status - 查看状态**

**为什么需要**：在保存之前，先看看改了什么文件。

**输出示例**：
```
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   src/login.js      ← 修改了但没添加到暂存区
        modified:   src/product.js    ← 修改了但没添加到暂存区

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        src/test.js                   ← 新文件，还没被Git跟踪
```

**状态说明**：
- `modified`：文件被修改了
- `Untracked`：新文件，Git还没跟踪
- `staged`：文件已添加到暂存区，准备提交

**步骤2：git diff - 查看修改内容**

**为什么需要**：看看具体改了什么，确认修改正确。

**输出示例**：
```diff
diff --git a/src/login.js b/src/login.js
index 1234567..abcdefg 100644
--- a/src/login.js
+++ b/src/login.js
@@ -10,6 +10,7 @@ function login(username, password) {
     // 验证用户名和密码
     if (validateUser(username, password)) {
+        console.log('登录成功');  ← 新增的代码（+号表示）
         return true;
     }
     return false;
```

**步骤3：git add - 添加到暂存区**

**为什么需要**：选择要保存的文件，不是所有修改都要保存。

**常用命令**：
```bash
# 添加单个文件
git add src/login.js

# 添加多个文件
git add src/login.js src/product.js

# 添加所有修改的文件（常用）
git add .

# 添加某个文件夹下的所有文件
git add src/
```

**注意事项**：
- `git add .` 会添加所有修改，包括你不想要的临时文件
- 建议先用 `git status` 看看，再选择性添加

**步骤4：git commit - 提交到仓库**

**为什么需要**：真正保存代码，创建一个版本快照。

**基本用法**：
```bash
git commit -m "提交说明"
```

**好的提交说明示例**：
```bash
# ✅ 好的提交说明（清晰描述做了什么）
git commit -m "添加用户登录功能"
git commit -m "修复商品列表分页bug"
git commit -m "优化支付接口性能"

# ❌ 不好的提交说明（太模糊）
git commit -m "修改"
git commit -m "更新"
git commit -m "fix"
```

**提交说明规范**：
```
格式：<类型>: <描述>

类型：
- feat: 新功能
- fix: 修复bug
- docs: 文档修改
- style: 代码格式（不影响功能）
- refactor: 重构代码
- test: 测试相关
- chore: 构建/工具相关

示例：
git commit -m "feat: 添加用户登录功能"
git commit -m "fix: 修复商品列表分页bug"
```

**步骤5：git push - 推送到远程仓库**

**为什么需要**：把本地保存的代码上传到服务器，让团队成员看到。

**基本用法**：
```bash
# 推送到远程仓库的main分支
git push origin main

# 如果设置了上游分支，可以简写
git push
```

**完整流程示例**：

假设你修改了登录功能：

```bash
# 1. 查看状态
git status
# 输出：modified: src/login.js

# 2. 查看修改内容（确认修改正确）
git diff src/login.js

# 3. 添加到暂存区
git add src/login.js

# 4. 提交
git commit -m "feat: 优化登录验证逻辑"

# 5. 推送到远程
git push origin main
```

### 4.3 查看历史记录

**为什么需要**：看看之前做了什么修改，什么时候改的。

#### git log - 查看提交历史

```bash
# 查看所有提交记录
git log

# 简洁版（一行显示）
git log --oneline

# 图形化显示分支
git log --oneline --graph --all

# 查看最近5条记录
git log -5
```

**输出示例**：
```
commit abc123def456 (HEAD -> main)
Author: 张三 <zhangsan@example.com>
Date:   Mon Jan 1 10:00:00 2024 +0800

    feat: 添加用户登录功能

commit 789ghi012jkl
Author: 李四 <lisi@example.com>
Date:   Sun Dec 31 15:30:00 2023 +0800

    fix: 修复商品列表bug
```

**说明**：
- `commit abc123`：提交的唯一ID（哈希值）
- `HEAD -> main`：当前所在的分支
- `Author`：谁提交的
- `Date`：什么时候提交的

#### git show - 查看某次提交的详情

```bash
# 查看最新提交的详情
git show

# 查看指定提交的详情
git show abc123
```

### 4.4 撤销操作（重要！）

#### 场景1：撤销工作区的修改（还没add）

**什么时候用**：改错了，想回到修改前的状态。

```bash
# 撤销单个文件的修改
git checkout -- 文件名

# 撤销所有文件的修改（危险！）
git checkout -- .
```

**为什么用checkout**：
- `checkout` 意思是"检出"，从仓库取出文件覆盖工作区
- `--` 表示后面是文件路径，不是分支名

**示例**：
```bash
# 你修改了login.js，但改错了
git checkout -- src/login.js
# login.js 恢复到修改前的状态
```

#### 场景2：撤销暂存区的文件（已经add，还没commit）

**什么时候用**：文件已经add了，但不想提交了。

```bash
# 从暂存区移除文件（但保留工作区的修改）
git reset HEAD 文件名

# 或使用新语法（Git 2.23+）
git restore --staged 文件名
```

**为什么用reset**：
- `reset` 意思是"重置"，把文件从暂存区移除
- 文件还在工作区，只是不在暂存区了

**示例**：
```bash
# 你add了login.js，但发现还有问题
git reset HEAD src/login.js
# login.js 从暂存区移除，但修改还在工作区
```

#### 场景3：撤销提交（已经commit了）

**什么时候用**：提交了但发现有问题，想撤销。

**方法1：撤销提交但保留修改（推荐）**

```bash
# 撤销最后一次提交，但保留修改
git reset --soft HEAD~1

# 然后可以重新修改，重新提交
git commit -m "新的提交说明"
```

**为什么用--soft**：
- `--soft`：只撤销提交，保留修改在暂存区
- `HEAD~1`：HEAD的上一个提交（最后一次提交）

**方法2：撤销提交并丢弃修改（危险！）**

```bash
# 撤销最后一次提交，并丢弃所有修改
git reset --hard HEAD~1
```

**⚠️ 警告**：`--hard` 会永久删除修改，无法恢复！

**方法3：创建新提交来撤销（推荐，如果已经push了）**

```bash
# 创建一个新提交，撤销之前的修改
git revert HEAD
```

**为什么用revert**：
- 如果已经push到远程仓库，用`revert`更安全
- 不会改变历史记录，只是添加一个新的提交来撤销

---

## 五、分支管理（团队协作必备）

### 5.1 为什么需要分支？

**场景**：
```
主分支（main）：稳定的代码，用户在使用
↓
你接到新任务：开发商品搜索功能
↓
如果直接在main分支开发：
- 代码可能有问题，影响主分支
- 其他开发者也在改代码，容易冲突
↓
解决方案：创建新分支
feature/search分支：独立开发搜索功能
↓
开发完成后，再合并到main分支
```

**分支的好处**：
- ✅ 主分支保持稳定
- ✅ 可以并行开发多个功能
- ✅ 出问题不影响主分支
- ✅ 可以随时切换分支

### 5.2 分支的基本操作

#### 查看分支

```bash
# 查看所有本地分支
git branch

# 查看所有分支（包括远程）
git branch -a

# 查看分支的详细信息
git branch -v
```

**输出示例**：
```
* main              ← *表示当前所在分支
  feature/login
  feature/product
```

#### 创建分支

```bash
# 创建新分支（但不切换）
git branch 分支名

# 创建并切换到新分支（常用）
git checkout -b 分支名

# 或使用新语法（Git 2.23+）
git switch -c 分支名
```

**为什么用checkout -b**：
- `checkout`：切换分支
- `-b`：创建新分支
- `checkout -b`：创建并切换

**示例**：
```bash
# 创建并切换到feature/login分支
git checkout -b feature/login

# 现在你在feature/login分支上
# 可以开始开发登录功能了
```

#### 切换分支

```bash
# 切换到已存在的分支
git checkout 分支名

# 或使用新语法
git switch 分支名
```

**注意事项**：
- 切换分支前，要提交或暂存当前修改
- 否则Git会阻止切换（防止丢失修改）

#### 删除分支

```bash
# 删除已合并的分支
git branch -d 分支名

# 强制删除分支（即使没合并）
git branch -D 分支名
```

**为什么用-d**：
- `-d`：安全删除，只有合并过的分支才能删除
- `-D`：强制删除，即使没合并也删除

### 5.3 合并分支（Merge）

**什么时候用**：功能开发完成，要把分支的代码合并到主分支。

#### 基本合并流程

```bash
# 1. 切换到主分支
git checkout main

# 2. 拉取最新代码（团队协作时）
git pull origin main

# 3. 合并功能分支
git merge feature/login

# 4. 如果有冲突，解决冲突后提交
# 5. 推送到远程
git push origin main
```

**为什么这样操作**：
1. 先切换到主分支：要在主分支上合并
2. 拉取最新代码：确保主分支是最新的
3. 合并分支：把功能分支的代码合并进来
4. 解决冲突：如果有冲突需要手动解决
5. 推送：把合并后的代码上传

#### 合并冲突处理

**什么时候会有冲突**：
```
主分支：修改了login.js的第10行
功能分支：也修改了login.js的第10行
↓
合并时Git不知道保留哪个，就会冲突
```

**冲突示例**：
```javascript
<<<<<<< HEAD
// 主分支的代码
function login() {
    console.log('主分支的登录');
}
=======
// 功能分支的代码
function login() {
    console.log('功能分支的登录');
}
>>>>>>> feature/login
```

**解决冲突的步骤**：
1. 打开冲突文件
2. 找到 `<<<<<<<`、`=======`、`>>>>>>>` 标记
3. 决定保留哪部分代码（或合并两部分）
4. 删除冲突标记
5. 保存文件
6. 添加到暂存区：`git add 文件名`
7. 完成合并：`git commit`

**示例**：
```javascript
// 解决后的代码（保留功能分支的版本）
function login() {
    console.log('功能分支的登录');
}
```

### 5.4 常见分支策略

#### Git Flow（推荐用于正式项目）

```
main分支：生产环境代码（稳定）
  │
  ├─ develop分支：开发环境代码
  │   │
  │   ├─ feature/功能名：新功能分支
  │   │
  │   └─ fix/bug名：修复bug分支
  │
  └─ release/版本号：发布准备分支
```

**工作流程**：
1. 从`develop`创建`feature`分支开发功能
2. 功能完成后合并回`develop`
3. 准备发布时创建`release`分支
4. 测试通过后合并到`main`和`develop`

#### 简化版（适合小团队）

```
main分支：主分支
  │
  ├─ feature/功能名：功能分支
  │
  └─ fix/bug名：修复分支
```

**工作流程**：
1. 从`main`创建`feature`分支
2. 开发完成后合并回`main`
3. 简单直接

---

## 六、远程仓库操作

### 6.1 什么是远程仓库？

**本地仓库 vs 远程仓库**：
```
本地仓库：在你的电脑上（.git文件夹）
  ↓ git push（推送）
远程仓库：在服务器上（GitHub/GitLab等）
  ↓ git pull（拉取）
本地仓库：更新代码
```

**为什么需要远程仓库**：
- ✅ 备份代码（电脑坏了也不怕）
- ✅ 团队协作（多人共享代码）
- ✅ 代码审查（Pull Request）
- ✅ 持续集成（CI/CD）

### 6.2 远程仓库的基本操作

#### 查看远程仓库

```bash
# 查看远程仓库列表
git remote

# 查看详细信息
git remote -v
```

**输出示例**：
```
origin  https://github.com/username/repo.git (fetch)
origin  https://github.com/username/repo.git (push)
```

**说明**：
- `origin`：远程仓库的默认名称
- `fetch`：拉取地址
- `push`：推送地址

#### 添加远程仓库

```bash
# 添加远程仓库
git remote add origin https://github.com/username/repo.git

# 添加其他远程仓库
git remote add upstream https://github.com/original/repo.git
```

**为什么叫origin**：
- `origin`是默认名称，可以改成其他名字
- 就像变量名，方便引用

#### 拉取代码（git pull）

**什么时候用**：从远程仓库下载最新代码。

```bash
# 拉取当前分支的最新代码
git pull origin main

# 如果设置了上游分支，可以简写
git pull
```

**pull做了什么**：
1. 从远程仓库下载代码（fetch）
2. 合并到当前分支（merge）

**等价操作**：
```bash
# 这两条命令等价于 git pull
git fetch origin main
git merge origin/main
```

#### 推送代码（git push）

**什么时候用**：把本地代码上传到远程仓库。

```bash
# 推送到远程仓库
git push origin main

# 如果设置了上游分支，可以简写
git push

# 第一次推送时设置上游分支
git push -u origin main
```

**为什么用-u**：
- `-u`（或`--set-upstream`）：设置上游分支
- 设置后，以后可以直接用`git push`，不用写分支名

#### 拉取和推送的完整流程

**日常协作流程**：

```bash
# 1. 开始工作前，先拉取最新代码
git pull origin main

# 2. 创建功能分支
git checkout -b feature/new-feature

# 3. 开发功能，提交代码
git add .
git commit -m "feat: 新功能"
git commit -m "fix: 修复bug"

# 4. 推送到远程（创建远程分支）
git push -u origin feature/new-feature

# 5. 在GitHub/GitLab创建Pull Request

# 6. 代码审查通过后，合并到main分支

# 7. 删除本地分支
git checkout main
git pull origin main  # 拉取合并后的代码
git branch -d feature/new-feature
```

### 6.3 克隆项目（git clone）

**什么时候用**：第一次获取项目代码。

```bash
# 克隆项目
git clone https://github.com/username/repo.git

# 克隆到指定文件夹
git clone https://github.com/username/repo.git my-project

# 克隆指定分支
git clone -b branch-name https://github.com/username/repo.git
```

**clone做了什么**：
1. 创建项目文件夹
2. 初始化Git仓库
3. 下载所有代码和历史记录
4. 自动设置远程仓库（origin）

---

## 七、常见问题处理

### 7.1 问题1：提交了错误的文件

**场景**：不小心提交了临时文件或敏感信息。

**解决方法**：

```bash
# 方法1：撤销最后一次提交，但保留修改
git reset --soft HEAD~1

# 删除不需要的文件
rm temp-file.txt

# 重新提交
git add .
git commit -m "正确的提交"
```

**如果已经push了**：

```bash
# 方法2：创建新提交来删除文件
git rm temp-file.txt
git commit -m "删除临时文件"
git push
```

### 7.2 问题2：提交信息写错了

**场景**：提交信息写错了，想修改。

**解决方法**：

```bash
# 修改最后一次提交的信息
git commit --amend -m "正确的提交信息"

# 如果已经push了，需要强制推送（谨慎！）
git push --force
```

**⚠️ 警告**：`--force`会覆盖远程历史，团队协作时要谨慎！

### 7.3 问题3：想撤销某个文件的修改

**场景**：只改错了一个文件，其他文件是对的。

**解决方法**：

```bash
# 撤销工作区的修改（还没add）
git checkout -- 文件名

# 撤销暂存区的文件（已经add）
git reset HEAD 文件名
git checkout -- 文件名

# 撤销某次提交中的文件（已经commit）
git checkout HEAD~1 -- 文件名
git commit -m "恢复文件"
```

### 7.4 问题4：合并冲突

**场景**：合并分支时出现冲突。

**解决方法**：

```bash
# 1. 查看冲突文件
git status

# 2. 打开冲突文件，找到冲突标记
# <<<<<<< HEAD
# 主分支的代码
# =======
# 功能分支的代码
# >>>>>>> feature/branch

# 3. 手动解决冲突（保留需要的代码，删除冲突标记）

# 4. 标记冲突已解决
git add 冲突文件

# 5. 完成合并
git commit
```

### 7.5 问题5：想回到之前的某个版本

**场景**：代码改乱了，想回到之前的版本。

**解决方法**：

```bash
# 1. 查看提交历史，找到要回到的版本
git log --oneline

# 2. 回到指定版本（但保留修改）
git reset --soft 提交ID

# 3. 回到指定版本（丢弃修改）
git reset --hard 提交ID
```

**⚠️ 警告**：`--hard`会永久删除修改！

### 7.6 问题6：忘记切换分支就修改代码

**场景**：应该在feature分支开发，但在main分支修改了。

**解决方法**：

```bash
# 1. 暂存当前修改
git stash

# 2. 切换到正确的分支
git checkout feature/branch

# 3. 恢复修改
git stash pop
```

**stash是什么**：
- 临时保存修改，不提交
- 可以随时恢复
- 适合切换分支时使用

### 7.7 问题7：想查看某个文件的修改历史

**场景**：想知道某个文件是谁改的，什么时候改的。

**解决方法**：

```bash
# 查看文件的提交历史
git log 文件名

# 查看文件的详细修改历史
git log -p 文件名

# 查看文件的每一行是谁改的
git blame 文件名
```

### 7.8 问题8：推送被拒绝（push rejected）

**场景**：推送时提示"rejected"，因为远程有新的提交。

**解决方法**：

```bash
# 1. 先拉取远程代码
git pull origin main

# 2. 如果有冲突，解决冲突
# 3. 重新推送
git push origin main
```

**为什么会被拒绝**：
- 远程仓库有新的提交
- Git防止覆盖别人的代码
- 必须先拉取，再推送

### 7.9 问题9：git diff显示"No newline at end of file"

**场景**：使用`git diff`查看修改时，最后显示`\ No newline at end of file`。

**原因**：
- 文件的最后一行没有换行符（newline character）
- 这是Git的提示信息，不是错误
- Unix/Linux系统约定文件末尾应该有换行符

**示例输出**：
```diff
diff --git a/src/login.js b/src/login.js
index 1234567..abcdefg 100644
--- a/src/login.js
+++ b/src/login.js
@@ -10,6 +10,7 @@ function login(username, password) {
     if (validateUser(username, password)) {
+        console.log('登录成功');
         return true;
     }
-    return false;
\ No newline at end of file
+    return false;
```

**解决方法**：

**方法1：手动添加换行符（推荐）**
```bash
# 1. 打开文件，在最后一行末尾按回车键添加换行符
# 2. 保存文件
# 3. 重新查看diff
git diff
```

**方法2：使用编辑器自动处理**
大多数现代编辑器（VS Code、Vim等）可以配置自动在文件末尾添加换行符：
- **VS Code**：设置中搜索"files.insertFinalNewline"，设置为`true`
- **Vim**：在`.vimrc`中添加`set eol`

**方法3：使用Git配置自动处理（全局）**
```bash
# 配置Git在提交时自动处理换行符
git config --global core.autocrlf true  # Windows
git config --global core.autocrlf input # Linux/Mac
```

**是否需要修复**：
- ✅ **建议修复**：符合Unix规范，避免某些工具出现问题
- ⚠️ **不是必须**：不影响代码功能，只是规范问题
- 📝 **团队协作**：如果团队有规范要求，应该统一修复

**批量修复所有文件**（如果需要）：
```bash
# 使用sed命令为所有文件添加换行符（Linux/Mac）
find . -type f -name "*.js" -exec sh -c 'if [ -n "$(tail -c 1 "$1")" ]; then echo >> "$1"; fi' _ {} \;

# Windows PowerShell
Get-ChildItem -Recurse -File | ForEach-Object {
    $content = Get-Content $_.FullName -Raw
    if ($content -and $content[-1] -ne "`n") {
        $content + "`n" | Set-Content $_.FullName -NoNewline
    }
}
```

**注意事项**：
- 修复后需要重新`git add`和`git commit`
- 如果文件已经提交，修复后会显示为修改（这是正常的）
- 团队协作时建议统一规范，避免频繁出现这个提示

---

## 八、最佳实践

### 8.1 提交规范

#### ✅ 好的提交习惯

1. **频繁提交**：完成一个小功能就提交一次
   ```bash
   # ✅ 好：每个功能一个提交
   git commit -m "feat: 添加登录验证"
   git commit -m "feat: 添加记住密码功能"
   
   # ❌ 不好：所有功能一次提交
   git commit -m "添加登录功能"
   ```

2. **清晰的提交信息**：
   ```bash
   # ✅ 好：清晰描述做了什么
   git commit -m "fix: 修复商品列表分页bug"
   
   # ❌ 不好：太模糊
   git commit -m "修改"
   ```

3. **提交前检查**：
   ```bash
   # 提交前先看看改了什么
   git status
   git diff
   ```

#### ❌ 避免的做法

1. **不要提交临时文件**：
   ```bash
   # 创建.gitignore文件，忽略临时文件
   # .gitignore
   node_modules/
   *.log
   .env
   ```

2. **不要提交敏感信息**：
   - 密码、API密钥等
   - 如果已经提交，立即修改并提交新版本

3. **不要提交大文件**：
   - 图片、视频等大文件用Git LFS或外部存储

### 8.2 分支管理规范

#### ✅ 好的分支命名

```bash
# 功能分支
feature/user-login
feature/product-search

# 修复分支
fix/login-bug
fix/payment-error

# 发布分支
release/v1.0.0

# 热修复分支
hotfix/critical-bug
```

#### ✅ 分支使用流程

1. **从main分支创建功能分支**
2. **在功能分支开发**
3. **完成后合并到main**
4. **删除功能分支**

### 8.3 团队协作规范

#### ✅ 协作流程

1. **开始工作前**：
   ```bash
   git pull origin main  # 拉取最新代码
   ```

2. **创建功能分支**：
   ```bash
   git checkout -b feature/my-feature
   ```

3. **开发过程中**：
   ```bash
   git add .
   git commit -m "feat: 功能描述"
   ```

4. **定期推送**：
   ```bash
   git push origin feature/my-feature
   ```

5. **完成后**：
   - 创建Pull Request
   - 代码审查
   - 合并到main分支

#### ✅ 避免冲突的方法

1. **经常拉取最新代码**：
   ```bash
   git pull origin main
   ```

2. **小功能快速完成**：减少冲突概率

3. **及时沟通**：和团队成员协调修改同一文件

### 8.4 .gitignore文件

**为什么需要**：告诉Git哪些文件不需要跟踪。

**创建.gitignore**：
```bash
# 在项目根目录创建.gitignore文件
touch .gitignore
```

**常见内容**：
```
# 依赖文件夹
node_modules/
vendor/

# 日志文件
*.log
logs/

# 环境变量文件
.env
.env.local

# 编辑器配置
.vscode/
.idea/
*.swp

# 操作系统文件
.DS_Store
Thumbs.db

# 构建产物
dist/
build/
*.min.js
```

### 8.5 常用命令速查

#### 日常开发

```bash
# 查看状态
git status

# 查看修改
git diff

# 添加文件
git add .

# 提交
git commit -m "提交信息"

# 推送
git push
```

#### 分支操作

```bash
# 查看分支
git branch

# 创建并切换分支
git checkout -b 分支名

# 切换分支
git checkout 分支名

# 合并分支
git merge 分支名

# 删除分支
git branch -d 分支名
```

#### 查看历史

```bash
# 查看提交历史
git log

# 简洁版
git log --oneline

# 图形化
git log --graph --oneline --all
```

#### 撤销操作

```bash
# 撤销工作区修改
git checkout -- 文件名

# 撤销暂存区
git reset HEAD 文件名

# 撤销提交
git reset --soft HEAD~1
```

---

## 九、总结

### 9.1 Git的核心概念

1. **三个区域**：工作区 → 暂存区 → 仓库
2. **提交**：保存代码的快照
3. **分支**：代码的平行宇宙
4. **远程仓库**：团队共享代码的地方

### 9.2 日常开发流程

```
1. git status        # 查看状态
2. git add .         # 添加到暂存区
3. git commit -m ""  # 提交到仓库
4. git push          # 推送到远程
```

### 9.3 团队协作流程

```
1. git pull          # 拉取最新代码
2. git checkout -b   # 创建功能分支
3. 开发功能
4. git commit        # 提交代码
5. git push          # 推送分支
6. 创建Pull Request
7. 代码审查
8. 合并到主分支
```

### 9.4 重要原则

1. **频繁提交**：小功能就提交
2. **清晰的信息**：提交信息要清楚
3. **先拉后推**：推送前先拉取
4. **使用分支**：新功能用新分支
5. **及时沟通**：团队协作要沟通

---

## 十、学习资源

### 10.1 官方文档

- Git官网：https://git-scm.com/
- Git文档：https://git-scm.com/doc
- Git教程：https://git-scm.com/docs/gittutorial

### 10.2 可视化工具

- **GitHub Desktop**：图形化Git工具（推荐新手）
- **SourceTree**：免费的Git GUI工具
- **VS Code Git插件**：编辑器内置Git功能

### 10.3 在线练习

- **Learn Git Branching**：https://learngitbranching.js.org/
- **GitHub Learning Lab**：https://lab.github.com/

### 10.4 参考书籍

- 《Pro Git》（免费在线版）：https://git-scm.com/book

---

**最后更新**：2024-01-01  
**维护者**：开发团队

**提示**：Git是一个工具，多练习才能熟练掌握。遇到问题不要慌，大部分操作都可以撤销！
