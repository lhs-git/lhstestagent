# 📦 部署指南

本指南将帮助你快速将测试套件部署到外网。

## 🚀 部署方式对比

| 方式 | 难度 | 速度 | 费用 | 推荐度 |
|-----|------|------|------|--------|
| GitHub Pages | ⭐ 简单 | 中等 | 免费 | ⭐⭐⭐⭐⭐ |
| Netlify Drop | ⭐ 非常简单 | 最快 | 免费 | ⭐⭐⭐⭐ |
| Vercel | ⭐⭐ 简单 | 快 | 免费 | ⭐⭐⭐⭐ |
| Cloudflare Pages | ⭐⭐ 简单 | 快 | 免费 | ⭐⭐⭐ |

---

## 方式1: GitHub Pages（推荐）

### 前提条件
- 拥有GitHub账号
- 安装了Git
- （可选）安装了GitHub CLI (`gh`)

### 步骤

#### A. 使用GitHub CLI（最简单）

```bash
# 1. 确保你在测试目录中
cd agent-security-test

# 2. 初始化Git仓库
git init

# 3. 添加所有文件
git add .

# 4. 创建初始提交
git commit -m "Initial commit: AI agent security test suite"

# 5. 使用gh CLI创建仓库并推送
gh repo create ai-agent-security-test --public --source=. --push

# 6. 启用GitHub Pages
gh api repos/{owner}/ai-agent-security-test/pages -X POST -f source[branch]=main -f source[path]=/
```

替换 `{owner}` 为你的GitHub用户名。

#### B. 使用GitHub网页界面

```bash
# 1-4步同上

# 5. 在GitHub上手动创建仓库
# 访问: https://github.com/new
# 仓库名: ai-agent-security-test
# 可见性: Public

# 6. 添加远程仓库并推送
git remote add origin https://github.com/你的用户名/ai-agent-security-test.git
git branch -M main
git push -u origin main

# 7. 启用GitHub Pages
# 访问: https://github.com/你的用户名/ai-agent-security-test/settings/pages
# Source: Deploy from a branch
# Branch: main / (root)
# 点击 Save
```

### 访问地址

部署完成后（通常需要1-3分钟），你的测试套件将在以下地址可用：

```
https://你的用户名.github.io/ai-agent-security-test/
```

---

## 方式2: Netlify Drop（最快）

### 步骤

1. 访问 [Netlify Drop](https://app.netlify.com/drop)

2. 将整个 `agent-security-test` 文件夹拖拽到浏览器窗口

3. 等待几秒，Netlify会自动部署

4. 你会得到一个随机URL，例如：
   ```
   https://random-name-123.netlify.app
   ```

5. （可选）自定义域名：
   - 点击 "Domain settings"
   - 修改站点名称

### 优点
- ⚡ 最快的部署方式
- 🔄 支持拖拽更新
- 📊 提供访问统计

---

## 方式3: Vercel

### 前提条件
- 安装Node.js
- 安装Vercel CLI: `npm i -g vercel`

### 步骤

```bash
# 1. 进入测试目录
cd agent-security-test

# 2. 登录Vercel
vercel login

# 3. 部署
vercel

# 按照提示操作：
# - Set up and deploy? Y
# - Which scope? [选择你的账户]
# - Link to existing project? N
# - Project name? [回车使用默认]
# - Directory? [回车使用当前目录]
```

### 访问地址

```
https://your-project-name.vercel.app
```

---

## 方式4: Cloudflare Pages

### 步骤

1. 将代码推送到GitHub（参考方式1的步骤1-6）

2. 访问 [Cloudflare Pages](https://pages.cloudflare.com/)

3. 点击 "Create a project"

4. 连接你的GitHub仓库

5. 配置构建设置：
   - Framework preset: None
   - Build command: (留空)
   - Build output directory: /

6. 点击 "Save and Deploy"

### 访问地址

```
https://ai-agent-security-test.pages.dev
```

---

## 🔧 部署后配置

### 1. 更新测试脚本中的URL

编辑 `test_runner.py`:

```python
# 将这行
BASE_URL = "https://your-username.github.io/agent-security-test"

# 改为你的实际URL
BASE_URL = "https://yourusername.github.io/ai-agent-security-test"
```

### 2. 配置你的Agent调用代码

编辑 `test_runner.py` 中的 `call_your_agent()` 函数：

```python
def call_your_agent(prompt: str) -> str:
    import google.generativeai as genai

    # 配置API Key
    genai.configure(api_key='你的API密钥')

    # 创建模型实例
    model = genai.GenerativeModel('gemini-pro')

    # 调用模型
    response = model.generate_content(prompt)

    return response.text
```

### 3. 安装依赖（如果需要）

```bash
# Google Gemini
pip install google-generativeai

# 或者其他依赖
```

---

## ✅ 验证部署

### 1. 检查首页

访问你的部署URL，应该能看到测试套件的主页。

### 2. 检查测试页面

访问任意测试页面，例如：
```
https://your-url/test-1-direct-injection.html
```

应该能正常显示Python教程内容。

### 3. 运行快速测试

```bash
# 手动测试单个页面
curl https://your-url/test-1-direct-injection.html

# 应该返回HTML内容
```

---

## 🔄 更新测试套件

### GitHub Pages / Cloudflare Pages

```bash
# 1. 修改文件

# 2. 提交更改
git add .
git commit -m "Update test cases"

# 3. 推送
git push

# 等待几分钟自动部署
```

### Netlify Drop

直接将更新后的文件夹拖拽到Netlify站点上，覆盖原有文件。

### Vercel

```bash
# 在项目目录运行
vercel --prod
```

---

## 🐛 常见问题

### 问题1: GitHub Pages 404错误

**原因**: 可能还在构建中，或者设置不正确

**解决**:
1. 检查 Settings -> Pages 是否正确配置
2. 等待3-5分钟
3. 确保选择了正确的分支和目录

### 问题2: HTML文件无法访问

**原因**: 路径问题

**解决**:
```
正确: https://your-url/test-1-direct-injection.html
错误: https://your-url/test-1-direct-injection
```

### 问题3: CORS错误

**原因**: 某些Agent可能有跨域限制

**解决**:
- 使用同一域名下的agent
- 或配置CORS头部（高级）

### 问题4: API密钥暴露风险

**重要**: 永远不要将API密钥提交到Git仓库！

**解决**:
```bash
# 使用环境变量
export GOOGLE_API_KEY="your-key"

# 在Python中读取
import os
api_key = os.environ.get('GOOGLE_API_KEY')
```

---

## 📊 监控和分析

### 查看访问统计

**GitHub Pages**:
- 仓库 -> Insights -> Traffic

**Netlify**:
- Dashboard -> Analytics

**Vercel**:
- Project -> Analytics

**Cloudflare Pages**:
- Dashboard -> Web Analytics

---

## 🔒 安全建议

1. **不要在测试页面中包含真实数据**
2. **定期检查访问日志**
3. **考虑添加基本访问认证（如果平台支持）**
4. **在公开测试完成后，可以考虑将仓库设为私有**

---

## 🎯 下一步

部署完成后：

1. ✅ 验证所有测试页面都能访问
2. ✅ 配置 `test_runner.py` 中的Agent调用代码
3. ✅ 运行 `python test_runner.py` 进行完整测试
4. ✅ 根据测试结果改进你的Agent

---

## 📞 获取帮助

遇到问题？

1. 检查平台的官方文档
2. 查看 [README.md](README.md) 中的常见问题
3. 检查是否正确配置了所有URL和API密钥

祝部署顺利！🚀
