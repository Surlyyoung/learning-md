---
title: Webpack
created: '2020-03-12T09:55:34.336Z'
modified: '2020-03-12T10:01:26.326Z'
---

# Webpack

### webpack-bundle-analyzer
参考[webpack-bundle-analyer](https://www.npmjs.com/package/webpack-bundle-analyzer)

1. 配合angular使用
- npm install --save-dev webpack-bundle-analyzer
- ng build --prod --stats-json // 生成json文件
- [npx] webpack-bundle-analyzer dist/stats.json // 分析;dist/stats.json为文件地址
