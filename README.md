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

例：
```
E:\WangAnStudy\file-cdn>tree /f
卷 新加卷 的文件夹 PATH 列表
卷序列号为 4EF4-FB8D
E:.
│  README.md
│
└─file
        第九周CTF练习题解.md
        第九周CTF练习题解.pdf
        第十周 CTF 题解.md
        第十周 CTF 题解.pdf


E:\WangAnStudy\file-cdn>git add .

E:\WangAnStudy\file-cdn>git commit -m "add files"
[main f7f2acc] add files
 5 files changed, 1871 insertions(+), 1 deletion(-)
 delete mode 100644 file
 create mode 100644 "file/\347\254\254\344\271\235\345\221\250CTF\347\273\203\344\271\240\351\242\230\350\247\243.md"
 create mode 100644 "file/\347\254\254\344\271\235\345\221\250CTF\347\273\203\344\271\240\351\242\230\350\247\243.pdf"
 create mode 100644 "file/\347\254\254\345\215\201\345\221\250 CTF \351\242\230\350\247\243.md"
 create mode 100644 "file/\347\254\254\345\215\201\345\221\250 CTF \351\242\230\350\247\243.pdf"

E:\WangAnStudy\file-cdn>git push origin main
Enumerating objects: 9, done.
Counting objects: 100% (9/9), done.
Delta compression using up to 24 threads
Compressing objects: 100% (7/7), done.
Writing objects: 100% (7/7), 44.22 MiB | 37.23 MiB/s, done.
Total 7 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To https://github.com/XVSHIFU/file-cdn.git
   6aaa34d..f7f2acc  main -> main

E:\WangAnStudy\file-cdn>
