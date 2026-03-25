# 🚀 Keli-Dog's Personal Blog

这是一个经过 **双分支架构优化** 和 **高定制化静态渲染** 的高效个人博客系统。

## 🌟 核心理念
- **源码与成品分离**：`hexo-source` 专注于“写”，`master` 专注于“看”。
- **精排渲染**：主页与核心子页面（如技术、项目、简历等）使用精排 HTML，确保 100% 样式兼容。

---

## 🛠️ 日常维护 (金标准工作流)

请按照以下两个步骤维护你的博客：

### 第一步：保存创作稿件 (在 `hexo-source` 分支)
写好内容后，先备份源码：
```powershell
git checkout hexo-source
git add .
git commit -m "保存底稿修改"
git push origin hexo-source
```

### 第二步：一键同步上线 (同步到 `master` 分支)
将 `public` 文件夹的内容完美同步到线上根目录。请**直接复制整段代码**到 PowerShell 运行：

```powershell
# 1. 生成网页预览并暂存到临时目录
npx hexo clean; npx hexo generate; Copy-Item -Path "public" -Destination "$env:TEMP\hexo_deploy_tmp" -Recurse -Force

# 2. 切换到部署分支（务必确保切换成功后再继续！）
git checkout master

# 3. 如果成功切换到 master，则清空旧版本并拷入新版本
if ($LASTEXITCODE -eq 0) {
    Get-ChildItem -Exclude .git | Remove-Item -Recurse -Force
    Copy-Item -Path "$env:TEMP\hexo_deploy_tmp\*" -Destination "." -Recurse -Force
    New-Item -ItemType File -Name ".nojekyll" -Force

    git add -A; git commit -m "全站同步更新: $(Get-Date -Format 'yyyy-MM-dd HH:mm')"; git push origin master -f
} else {
    Write-Host "切换 master 分支失败，已终止部署以免误删源码！" -ForegroundColor Red
}

# 4. 清理临时目录并切回源码分支
Remove-Item -Path "$env:TEMP\hexo_deploy_tmp" -Recurse -Force
git checkout hexo-source
```

---

## 📂 结构说明
- **`source/`**: 绝对不能丢的原始稿件库。
- **`index.html` (根目录)**: 纯手动定制的动态卡片首页，非常重要。
- **`_config.yml`**: 全局配置，控制着 `skip_render` 等核心逻辑。

---
*Created with ❤️ by Antigravity*
