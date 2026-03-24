# 🚀 Keli-Dog's Personal Blog

这是一个基于 **Hexo** 框架构建，并经过 **双分支架构优化** 和 **高定制化静态渲染** 的个人博客系统。

## 🌟 项目亮点

-   **沉浸式卡片首页**：采用独立设计的 `index.html`，提供 6 个核心板块的动态卡片入口。
-   **双分支管理策略**：
    -   `hexo-source`：源码分支，存放所有配置文件、原始页面和高清素材。
    -   `master`：部署分支，仅存放生成的静态网页，确保 GitHub Pages 高效加载。
-   **精排静态子页面**：由于 Node 24 与旧版 Hexo 渲染引擎存在兼容性差异，本项目采用了“精排静态 HTML”方案，确保所有子页面（技术、项目等）拥有完美样式的同时，不再依赖复杂且易碎的渲染过程。
-   **.nojekyll 支持**：完美避开 GitHub Pages 默认的渲染干扰。

## 📂 目录结构

### `hexo-source` 分支 (源码层)
-   `_config.yml`: Hexo 全局配置文件（已配置 `skip_render` 保护静态页面）。
-   `package.json`: 项目依赖包管理。
-   `index.html`: 卡片主页源码。
-   `source/`: **核心创作目录**。
    -   `img/`: 存放头像等资源。
    -   `tech-articles/`, `projects/`, ... : 各板块目录，内含 `index.html` 精排稿。

### `master` 分支 (生产层)
-   网站根目录下仅保留生成的 HTML/CSS/JS 以及资源文件夹，是最终用户访问看到的部分。

## 🛠️ 日常维护流程

### 1. 修改/增加内容
进入 `hexo-source` 分支，直接修改 `source/` 下对应板块的 `index.html`：
```bash
git checkout hexo-source
# 修改 source/tech-articles/index.html ...
```

### 2. 预览效果 (可选)
运行 Hexo 本地服务器查看效果：
```bash
npx hexo s
# 访问 http://localhost:4000
```

### 3. 同步到正式环境 (一键部署命令)
为了确保部署过程不丢失文件，请直接在 PowerShell 中复制并运行以下**整段逻辑**：

```powershell
# 1. 确保在源码分支并生成最新网页
git checkout hexo-source; npx hexo clean; npx hexo generate

# 2. 将生成好的网页暂时存放到系统临时文件夹
Copy-Item -Path "public" -Destination "$env:TEMP\hexo_deploy_tmp" -Recurse -Force

# 3. 切换到部署分支并清空旧内容 (保留 .git 记录)
git checkout master
Get-ChildItem -Exclude .git | Remove-Item -Recurse -Force

# 4. 从临时文件夹拷回成品并添加上线标记
Copy-Item -Path "$env:TEMP\hexo_deploy_tmp\*" -Destination "." -Recurse -Force
New-Item -ItemType File -Name ".nojekyll" -Force

# 5. 提交并强制推送到 GitHub
git add -A
git commit -m "Site Update: $(Get-Date -Format 'yyyy-MM-dd HH:mm')"
git push origin master -f

# 6. 清理临时工并切回源码分支
Remove-Item -Path "$env:TEMP\hexo_deploy_tmp" -Recurse -Force
git checkout hexo-source
```

## 🛡️ 注意事项
-   **不要在 `master` 分支修改代码**：所有改动请务必在 `hexo-source` 进行，否则下次部署会被覆盖。
-   **图片资源**：请放入 `source/img/` 或 `source/images/`，并在 HTML 中使用相对路径引用。

---
*Created with ❤️ by Antigravity*
