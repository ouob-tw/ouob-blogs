---
title: Blogging - Obsidian to Hugo
date: 2025-04-28
draft: false
tags:
  - obsidian
  - hugo
---
💡 此文章主要是紀錄建站過程，不是教學。

目標：
- 在obsidian寫作；博客網站展示
- 一鍵發布文章（自動化流程）

前置要求：
- 筆者使用macOS
- 已安裝好docker
- 遠端Linux主機的SSH
- 遠端Linux主機公開的port端口

主要內容參考 [I started a blog.....in 2024 (why you should too) - YouTube](https://www.youtube.com/watch?v=dnE7c0ELEH8)。影片中使用Hostinger，本文使用自己主機建置。

## 安裝與設定hugo

prepare a folder in obsidian for blog posts
```python
~/Resilio/Obsidian/ouob_posts/
```

install hugo (macOS)
```bash
brew install hugo
```

generate site config
```sh
hugo new site <path>
```
result:
```bash
rrrfff@mbp ouob-hugo % hugo new site .
Congratulations! Your new Hugo site was created in /Users/rrrfff/ouob-blogs.

Just a few more steps...

1. Change the current directory to /Users/rrrfff/ouob-hugo.
2. Create or install a theme:
   - Create a new theme with the command "hugo new theme <THEMENAME>"
   - Or, install a theme from https://themes.gohugo.io/
3. Edit hugo.toml, setting the "theme" property to the theme name.
4. Create new content with the command "hugo new content <SECTIONNAME>/<FILENAME>.<FORMAT>".
5. Start the embedded web server with the command "hugo server --buildDrafts".

See documentation at https://gohugo.io/.
```

create git repo in hugo site path
```sh
git init
```

add theme to our hugo site
[Hugo Themes](https://themes.gohugo.io/)

In this demo i use PaperMod theme
```
git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
```
> Git submodule 是 Git 版本控制系統中的一個功能，它允許你將一個 Git 儲存庫作為另一個 Git 儲存庫的子目錄。這樣做的主要好處是可以在你的專案中包含和使用其他專案的程式碼，同時保持兩個專案的獨立性。

<details>  
<summary>Click to expand</summary>
這個命令的作用是：
1. 將 hugo-PaperMod 這個 Hugo 主題的 Git 儲存庫添加為你當前專案的一個子模組
2. 子模組會被克隆到 themes/PaperMod 目錄下
3. `--depth=1` 參數表示只克隆最新的一個版本的代碼，不包含完整的歷史記錄，這樣可以節省空間和下載時間

使用 submodule 的優點：
- 可以在你的專案中使用其他專案的程式碼，而不需要複製貼上或手動更新
- 當原始專案（在這個例子中是 hugo-PaperMod 主題）更新時，你可以輕鬆地更新你的子模組
- 你可以保持對使用的特定版本的控制
</details>

參考[papermod的wiki](https://github.com/adityatelange/hugo-PaperMod/wiki/Installation#sample-hugoyml) 來安裝

記得如果複製裡面的範例設定檔 `hugo.toml`要移除--`paginate = 5`，因hugo已經棄用。

筆者還是沿用toml格式
[YAML to TOML](https://transform.tools/yaml-to-toml)

start hugo dev server
```sh
hugo server
```

create folder for posts 
```sh
mkdir ~/ouob-hugo/content/posts
```

sync path with obsidian 
```sh
rsync -av --delete ~/Resilio/Obsidian/ouob_posts/ ~/ouob-hugo/content/posts/
```
`--delete` 會刪除目標目錄中存在，但在來源目錄中不存在的文件。


add title, date and tag for obsidian article
```
---
title: blogtitle
date: 2024-11-06
draft: false
tags:
  - tag1
  - tag2
---
```

創建圖片腳本：將圖片一併複製到hugo專案路徑
https://blog.networkchuck.com/posts/my-insane-blog-pipeline/#maclinux-1


參考資料：
[I started a blog.....in 2024 (why you should too) - YouTube](https://www.youtube.com/watch?v=dnE7c0ELEH8)

hugo docker version choice
https://docker.hugomods.com/zh-hant/docs/tags/
開發時：不懂的話直接選 [exts](https://docker.hugomods.com/zh-hant/docs/tags/#exts)
```sh
docker pull hugomods/hugo:exts
```
生產環境部屬時 選`hugomods/hugo:nginx`
[使用 Nginx 鏡像部署 - CI/CD - 文檔 - Hugo Docker 鏡像 | HugoMods](https://docker.hugomods.com/zh-hant/docs/ci-cd/nginx/)