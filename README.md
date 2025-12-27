# file-cdn

这是一个用于存放静态资源的仓库，通过 jsDelivr 提供 CDN 访问。

## 使用方式

### PDF
https://cdn.jsdelivr.net/gh/XVSHIFU/file-cdn@main/files/pdf/xxx.pdf

### ZIP
https://cdn.jsdelivr.net/gh/XVSHIFU/file-cdn@main/files/zip/xxx.zip


## 注意事项
- 单文件建议 ≤ 20MB
- 文件名仅使用英文、数字、-、_
- 更新文件请更换文件名，避免 CDN 缓存


## 本地文件提交

① 添加所有变更
git add .

② 正确提交（必须加 -m）
git commit -m "add week10 files"

③ 推送
git push origin main
