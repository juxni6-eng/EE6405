# AI-Contrast Lab Site, with Landing + Dashboard

## How to run in PyCharm
1. Open this folder in PyCharm.
2. Open Terminal, run:
   npm install
   npm run dev
3. Visit the printed local URL.
4. Edit `src/pages/Home.vue` 填写小组与项目简介。
5. 指标文件放 `src/data/metrics.json`，图片放 `public/images/...`。
6. 构建静态文件：
   npm run build
   # 部署 dist/ 到 GitHub Pages 或 Netlify

## 路由
- `/` 主页，展示小组和项目介绍，中央按钮跳转到 Dashboard
- `/dashboard` 成果展示页，含模型、数据集选择，指标表与图像区

## 自定义建议
- 在 `Home.vue` 中把三个卡片换成你的内容，插入合作单位Logo、论文链接等。
- 想要更“动效”，可以引入 `@vueuse/motion` 或直接用 CSS transition。
