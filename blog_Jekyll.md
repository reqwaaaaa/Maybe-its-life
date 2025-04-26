# GitHub Pages + Jekyll 个人博客完整搭建与部署方案

---

## 1. 当前状态概述

- 已有 GitHub 仓库：`reqwaaaaa.github.io`
- GitHub Pages 功能已激活
- 本地尚未配置 Ruby/Jekyll 环境
- 目标：搭建一个漂亮、可本地维护、基于 Jekyll 的完整个人博客，未来可以本地编辑 Markdown，git 推送更新网页

---

## 2. 本地开发环境安装（macOS + zsh）

### 2.1 安装 Homebrew（若未安装）

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 2.2 安装 Ruby

```bash
brew install ruby
```

添加 Ruby 到环境变量（编辑 ~/.zshrc）：

```bash
echo 'export PATH="/opt/homebrew/opt/ruby/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

检查 Ruby 安装：

```bash
ruby -v
```

### 2.3 安装 Bundler 和 Jekyll

```bash
gem install --user-install bundler jekyll
```

检查 Jekyll 安装：

```bash
jekyll -v
```

---

## 3. 本地初始化博客项目

### 3.1 克隆 GitHub 仓库

```bash
git clone https://github.com/reqwaaaaa/reqwaaaaa.github.io.git
cd reqwaaaaa.github.io
```

### 3.2 生成 Jekyll 标准骨架

```bash
jekyll new . --force
```

覆盖当前目录生成标准博客结构。

### 3.3 安装依赖

```bash
bundle install
```

---

## 4. 本地开发与预览

本地运行：

```bash
bundle exec jekyll serve
```

浏览器访问：

```
http://localhost:4000
```

修改 Markdown 文件可实时预览。

---

## 5. 更换专业主题（Minimal Mistakes）

### 5.1 修改 `_config.yml`

新增或更新以下配置项：

```yaml
remote_theme: mmistakes/minimal-mistakes
plugins:
  - jekyll-include-cache
```

### 5.2 安装主题依赖

```bash
bundle add jekyll-include-cache
bundle install
```

---

## 6. GitHub Pages 部署

### 6.1 提交并推送更新

```bash
git add .
git commit -m "初始化 Jekyll 博客框架并配置主题"
git push origin main
```

GitHub Pages 将自动拉取并部署，无需手动构建。

### 6.2 检查网页访问

访问：

```
https://reqwaaaaa.github.io/
```

---

## 7. 后期维护与更新流程

- 本地新建 Markdown 文章文件，放在 `_posts/` 目录，文件命名格式为 `YYYY-MM-DD-title.md`
- 本地 `bundle exec jekyll serve` 预览
- 修改完成后：

```bash
git add .
git commit -m "新增文章或更新内容"
git push origin main
```

- 线上网页自动更新，无需任何额外操作。

---

## 8. 附加增强功能

- 增加 sitemap.xml：利于 SEO 收录
- 增加 Google Analytics：网站访问统计
- 增加 RSS feed：订阅推送支持
- 增加标签分类页面
- 配置 About 页面、自定义 404 页面
- 配置项目子域名（如 blog.reqwaaaaa.com）（需自购域名）

---

## 9. 注意事项总结

- Ruby 版本需在 2.5 及以上
- `.zshrc` 中 PATH 设置正确
- 仓库名与用户名一致（reqwaaaaa.github.io）
- `_posts/` 文件严格遵循时间戳命名规范
- Minimal Mistakes 主题使用 remote_theme，需要在 GitHub Pages 允许 `build with GitHub Actions`
- GitHub Pages 自动部署默认延迟约 10~60 秒，不需要手动 build

---

## 10. 项目初始结构示例

```text
reqwaaaaa.github.io/
├── _config.yml
├── _posts/
│   └── 2024-05-11-hello-world.md
├── _layouts/
├── _includes/
├── _sass/
├── assets/
├── index.md
├── about.md
├── 404.html
└── README.md
```

---

## 11. 后续支持事项

- 完成 `_config.yml` 全配置
- 生成首页、About页、分类目录页
- 生成初版 `_posts` 示例文章
- 配置 Minimal Mistakes 的 YAML 结构
- 本地优化 Jekyll Serve 测速
- 指导如何使用 git 管理文章增删改查
- 持续优化 CSS 样式 / 添加个人 Logo
- 指导绑定自定义域名（如果后续需要）

---