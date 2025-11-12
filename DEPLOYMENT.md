# 网站部署指南 (Deployment Guide)

本文档提供详细的网站部署步骤和最佳实践。

## 📋 部署前检查清单

在部署之前，请确保完成以下所有项目：

### 1. 内容更新
- [ ] 替换所有占位符图片为实际产品照片
- [ ] 更新公司名称和品牌信息
- [ ] 添加真实的联系方式（电话、邮箱、地址）
- [ ] 检查所有文本内容的准确性
- [ ] 更新客户案例和推荐语
- [ ] 确认社交媒体链接正确

### 2. 图片优化
- [ ] 所有图片已压缩（使用 TinyPNG 或类似工具）
- [ ] Hero 图片 < 500KB
- [ ] 案例图片 < 200KB
- [ ] 图片格式正确（JPG/WebP）
- [ ] 所有图片包含 ALT 标签（SEO）

### 3. 功能测试
- [ ] 所有链接正常工作
- [ ] 联系表单能正常提交
- [ ] 移动端菜单正常展开/收起
- [ ] 案例筛选功能正常
- [ ] 所有按钮有响应
- [ ] 页面滚动流畅

### 4. 跨浏览器测试
- [ ] Chrome 测试通过
- [ ] Firefox 测试通过
- [ ] Safari 测试通过
- [ ] Edge 测试通过
- [ ] 移动浏览器测试通过

### 5. SEO 优化
- [ ] 页面标题包含关键词
- [ ] Meta 描述已设置（120-155字符）
- [ ] 所有图片有 ALT 属性
- [ ] H1, H2, H3 标签正确使用
- [ ] URL 结构清晰

---

## 🚀 部署方法

### 方法 1: Netlify (推荐 - 最简单)

**优点：** 免费、自动 HTTPS、全球 CDN、持续部署

**步骤：**

1. 访问 https://netlify.com 并注册账号

2. 点击 "Add new site" → "Deploy manually"

3. 将整个项目文件夹拖拽到上传区域

4. 等待部署完成（通常 < 1分钟）

5. 你的网站将获得一个类似 `your-site-name.netlify.app` 的 URL

6. （可选）绑定自定义域名：
   - 在 Netlify 控制台点击 "Domain settings"
   - 点击 "Add custom domain"
   - 按照提示更新你的 DNS 设置

**自动部署（进阶）：**
```bash
# 安装 Netlify CLI
npm install -g netlify-cli

# 登录
netlify login

# 初始化项目
netlify init

# 部署
netlify deploy --prod
```

---

### 方法 2: Vercel

**优点：** 快速、免费、优秀的性能

**步骤：**

1. 访问 https://vercel.com 并注册

2. 点击 "New Project"

3. 上传项目文件或连接 Git 仓库

4. 配置设置：
   - Framework Preset: Other
   - Root Directory: `./`
   - Build Command: (留空)
   - Output Directory: `./`

5. 点击 "Deploy"

6. 部署完成后，你会获得一个 `.vercel.app` 域名

---

### 方法 3: GitHub Pages (免费)

**优点：** 与 Git 集成、免费、适合开源项目

**步骤：**

1. 在 GitHub 创建新仓库

2. 上传项目文件：
```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/你的用户名/仓库名.git
git push -u origin main
```

3. 在仓库设置中：
   - 进入 Settings → Pages
   - Source 选择 "main" 分支
   - Root 选择 "/ (root)"
   - 点击 Save

4. 等待几分钟，网站将在 `https://你的用户名.github.io/仓库名/` 上线

---

### 方法 4: Cloudflare Pages

**优点：** 超快速度、免费 SSL、强大的 CDN

**步骤：**

1. 访问 https://pages.cloudflare.com

2. 连接你的 Git 账号（GitHub/GitLab）

3. 选择仓库并配置：
   - Build command: (留空)
   - Build output directory: `/`

4. 点击 "Save and Deploy"

---

### 方法 5: 传统虚拟主机（通过 FTP）

**适用于：** 已有虚拟主机服务的用户

**步骤：**

1. 使用 FTP 客户端（如 FileZilla）连接到服务器

2. 连接信息通常包括：
   - 主机地址：ftp.yourdomain.com
   - 用户名：你的 FTP 用户名
   - 密码：你的 FTP 密码
   - 端口：21 (FTP) 或 22 (SFTP)

3. 导航到网站根目录（通常是 `public_html` 或 `www`）

4. 上传所有文件：
   - index.html
   - css/ 文件夹
   - js/ 文件夹
   - images/ 文件夹

5. 设置文件权限：
   - 文件：644
   - 文件夹：755

6. 访问你的域名检查网站

---

## 🔧 部署后配置

### 1. 设置联系表单

**使用 EmailJS (无需后端):**

```html
<!-- 在 index.html 的 </body> 前添加 -->
<script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@3/dist/email.min.js"></script>
<script>
  emailjs.init("你的_PUBLIC_KEY");
</script>
```

在 `js/main.js` 的表单提交部分替换为：
```javascript
emailjs.sendForm('你的_SERVICE_ID', '你的_TEMPLATE_ID', contactForm)
  .then(() => {
    alert('感谢您的咨询！我们将在24小时内回复。');
    contactForm.reset();
  }, (error) => {
    alert('发送失败，请稍后再试或直接联系我们。');
  });
```

**注册 EmailJS：**
1. 访问 https://www.emailjs.com/
2. 注册免费账号（每月 200 封邮件免费）
3. 添加邮件服务（Gmail、Outlook 等）
4. 创建邮件模板
5. 获取 Public Key、Service ID、Template ID

---

### 2. 添加 Google Analytics

在 `index.html` 的 `<head>` 中添加：

```html
<!-- Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_MEASUREMENT_ID');
</script>
```

**获取 Measurement ID：**
1. 访问 https://analytics.google.com/
2. 创建账号和属性
3. 获取 Measurement ID (格式: G-XXXXXXXXXX)

---

### 3. 设置 Google Search Console

1. 访问 https://search.google.com/search-console

2. 添加网站：
   - 输入你的网站 URL
   - 验证所有权（HTML 文件验证或 Meta 标签验证）

3. 提交 sitemap：
   - 创建 `sitemap.xml` 文件
   - 在 Search Console 中提交

**简单的 sitemap.xml：**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://yourdomain.com/</loc>
    <lastmod>2025-11-11</lastmod>
    <changefreq>monthly</changefreq>
    <priority>1.0</priority>
  </url>
</urlset>
```

---

### 4. 配置 SSL 证书（HTTPS）

**如果使用 Netlify/Vercel/GitHub Pages/Cloudflare：**
- SSL 自动配置，无需手动操作

**如果使用传统虚拟主机：**
- 联系主机商启用 Let's Encrypt 免费 SSL
- 或通过主机控制面板（cPanel）自行启用

---

### 5. 性能优化

**启用压缩（如果使用传统主机）：**

创建 `.htaccess` 文件：
```apache
# 启用 Gzip 压缩
<IfModule mod_deflate.c>
  AddOutputFilterByType DEFLATE text/html text/plain text/xml text/css text/javascript application/javascript
</IfModule>

# 浏览器缓存
<IfModule mod_expires.c>
  ExpiresActive On
  ExpiresByType image/jpg "access plus 1 year"
  ExpiresByType image/jpeg "access plus 1 year"
  ExpiresByType image/png "access plus 1 year"
  ExpiresByType image/webp "access plus 1 year"
  ExpiresByType text/css "access plus 1 month"
  ExpiresByType application/javascript "access plus 1 month"
</IfModule>
```

---

## 📊 部署后监控

### 1. 性能检查

**Google PageSpeed Insights:**
- 访问：https://pagespeed.web.dev/
- 输入你的网站 URL
- 目标分数：90+ (移动端和桌面端)

**改进建议：**
- 压缩图片
- 启用浏览器缓存
- 使用 CDN
- 延迟加载图片

### 2. 移动端友好性测试

- 访问：https://search.google.com/test/mobile-friendly
- 输入你的 URL
- 确保通过测试

### 3. 链接检查

使用工具检查断链：
- https://validator.w3.org/checklink
- 或使用 Screaming Frog SEO Spider

---

## 🔄 持续更新

### 定期维护任务：

**每月：**
- [ ] 添加新的客户案例
- [ ] 更新博客内容（如果有）
- [ ] 检查联系表单功能
- [ ] 查看 Google Analytics 数据

**每季度：**
- [ ] 更新产品图片
- [ ] 检查所有链接
- [ ] 审查 SEO 表现
- [ ] 更新客户推荐语

**每年：**
- [ ] 更新版权年份
- [ ] 全面审查内容准确性
- [ ] 评估网站改版需求
- [ ] 更新 SSL 证书（如果需要）

---

## ❓ 常见问题

### Q: 网站部署后显示 404 错误？
**A:** 检查文件名是否正确（index.html），确保上传到正确的目录。

### Q: 图片不显示？
**A:** 检查图片路径是否正确，确保 images 文件夹已上传。

### Q: 移动端菜单不工作？
**A:** 确保 JavaScript 文件已正确加载，检查浏览器控制台是否有错误。

### Q: 表单提交后没有反应？
**A:** 需要配置表单后端处理（EmailJS 或 PHP），参考上方"设置联系表单"部分。

### Q: 如何绑定自定义域名？
**A:** 在域名注册商处更新 DNS 记录，指向托管平台提供的地址。

---

## 📞 技术支持

如需帮助，可以：
1. 查看 README.md 文档
2. 检查代码注释
3. 咨询你的虚拟主机提供商
4. 联系网站开发人员

---

**部署快乐！** 🚀

如有任何问题，请参考主 README.md 或联系技术支持。

