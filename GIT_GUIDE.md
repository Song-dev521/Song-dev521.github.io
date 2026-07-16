
#“小微智管”项目 Git 协作规范 v1.0

> 适用团队：赵、宋、蒋、王、沙
> 适用项目：小微智管——面向中小微实体商户的低代码AI进销存系统


##一、分支策略（三分支模型）

| 分支名 | 用途 | 说明 |
| :--- | :--- | :--- |
| `master` | 生产环境稳定版 | 只接受来自 `dev` 的合并，**禁止直接提交** |
| `dev` | 开发主分支 | 所有功能分支最终都合并到这里 |
| `feature/xxx` | 功能分支 | 每人从 `dev` 拉出，开发完合并回 `dev` |

```
master
  ↑
dev   ←  feature/商品管理  (王)
  ↑
  ├── feature/NLU解析   (蒋)
  ├── feature/Prompt设计 (宋)
  └── feature/调研文档   (沙)
```

---

## 二、分支命名规范

功能分支统一用 `feature/` 前缀，后面跟功能名称（用短横线连接，全部小写）：

| 功能分支示例 | 说明 |
| :--- | :--- |
| `feature/product-manage` | 商品管理功能 |
| `feature/purchase-order` | 进货单功能 |
| `feature/nlu-parser` | 自然语言解析引擎 |
| `feature/prompt-template` | AI Prompt 模板 |
| `feature/dashboard` | 看板报表 |

> 格式：`feature/功能名`，用英文，小写，短横线连接。

---

## 三、Commit 规范

每次提交时，commit message 按以下格式写：

```
<type>(<scope>): <简短描述>
```

### 常见 type 类型

| type | 说明 | 示例 |
| :--- | :--- | :--- |
| `feat` | 新增功能 | `feat(商品): 添加商品分页查询` |
| `fix` | 修复Bug | `fix(进货): 修复进货单金额计算错误` |
| `docs` | 文档更新 | `docs: 更新API接口文档` |
| `style` | 代码格式调整 | `style: 统一缩进为2空格` |
| `refactor` | 重构代码（不改功能） | `refactor(NLU): 优化别名匹配逻辑` |
| `test` | 增加或修改测试 | `test: 添加商品解析单元测试` |
| `chore` | 构建/工具配置变更 | `chore: 更新Maven依赖版本` |

### scope 可选

填写你修改的模块，比如 `商品`、`进货`、`AI`、`报表`、`库存`、`赊账` 等。

### ✅ 好例子

```
feat(商品): 实现商品列表按分类筛选
fix(库存): 修复销售后库存未扣减的Bug
docs: 补充自然语言记账接口说明
```

### ❌ 坏例子

```
修改
更新代码
fix bug
```

---

## 四、协作流程

### 每日开始工作前

```bash
git checkout dev
git pull origin dev
git checkout -b feature/你的功能名
```

### 开发完成，准备提交

```bash
git add .
git commit -m "feat(模块): 功能描述"
git push origin feature/你的功能名
```

### 功能完成后，发起合并

在 Gitee 上创建 **Pull Request (PR)**，从 `feature/xxx` → `dev`，指定至少一位队友审核。

> 每次 PR 建议不超过 200 行变更，方便Code Review。

### 合并后，删除已合并的功能分支

```bash
git branch -d feature/你的功能名
git push origin --delete feature/你的功能名
```

---

## 五、禁止事项（红线）

| 禁止操作 | 原因 |
| :--- | :--- |
| ❌ 禁止直接提交到 `master` | 破坏稳定版本 |
| ❌ 禁止直接提交到 `dev` | 应该通过功能分支合并 |
| ❌ 禁止提交大文件（>50MB） | Git不适合存大文件，用云盘 |
| ❌ 禁止提交包含密码/密钥的代码 | 用 `.env` 或配置文件排除 |
| ❌ 禁止 `git push --force` 到公共分支 | 会覆盖队友代码 |

---

## 六、.gitignore 配置

确保项目根目录的 `.gitignore` 包含以下内容：

```
# IDEA
.idea/
*.iml

# VS Code
.vscode/

# Maven
target/
pom.xml.tag
pom.xml.releaseBackup
pom.xml.versionsBackup

# Node
node_modules/
dist/
.DS_Store

# 日志
*.log

# 本地环境配置（含密码）
application-dev.yml
application-local.yml
.env.local
*.secret

# 临时文件
*.tmp
*.swp
*.bak
```

---

## 七、常用命令速查

| 操作 | 命令 |
| :--- | :--- |
| 拉取最新dev代码 | `git pull origin dev` |
| 创建新功能分支 | `git checkout -b feature/xxx` |
| 查看当前分支 | `git branch` |
| 提交代码 | `git add . && git commit -m "feat: 描述"` |
| 推送到远程 | `git push origin feature/xxx` |
| 切换到dev | `git checkout dev` |
| 合并功能分支到dev | `git merge feature/xxx` |
| 删除本地功能分支 | `git branch -d feature/xxx` |

---

## 八、团队Git工作流（简化版）

```
1. 早上到工位 → git checkout dev → git pull
2. 开始写代码 → git checkout -b feature/你的功能
3. 写完了 → git add . → git commit -m "feat: 功能描述"
4. 推上去 → git push origin feature/你的功能
5. 在网页上发起 PR → 队友审核 → 合并到 dev
6. 删掉功能分支 → git branch -d feature/你的功能
```

---

## 九、责任人分工

| 职责 | 负责人 |
| :--- | :--- |
| 代码审核（后端/Java） | 蒋、赵 |
| 代码审核（前端/Vue） | 王 |
| 代码审核（AI/Prompt） | 宋 |
| PR 最终合并到 dev | 赵 |
| 维护 Git 规范执行 | 赵 |

---

*本规范自 2026年7月16日起执行，如有调整需经团队全员同意后更新版本。*
```