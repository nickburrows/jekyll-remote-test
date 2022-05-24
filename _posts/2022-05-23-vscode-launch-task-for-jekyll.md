---
title: "在VS Code中快速啟動Jekyll專案"
date: 2022-05-23
categories:
  - Jekyll
tags:
  - vscode
  - jekyll
---
<!-- markdownlint-disable MD031 -->
## 原因

複製 <span class="material-icons">content_paste</span>

已經習慣將VS Code作為日常使用的一個工具，常會遇到許多不適用的情況發生，而在執行一些Jekyll專案類型時，會需要在本機端運行 `bundle exec jekyll serve` 來檢視網站或是設定更改後的變化，但是每次都要輸入指令什麼的真的太累人了，找了一下官方的插件庫，有找到一個名為 [Jekyll Run](https://marketplace.visualstudio.com/items?itemName=Dedsec727.jekyll-run) 的擴充程式，但是由於該套件執行指令時判斷的路徑與我實際使用的不符，且無法自定路徑等只能透過另開一個視窗 (工作區) ，並在該工作區新增要運行的主目錄才能正確運行。

於是上網找了一些方法，後來決定使用VS Code內建的debug功能來解決這個問題。

## 如何實現

在要運行指令的目錄下新增 `.vscode` 資料夾 (用來放和VS Code一些設定有關的檔案)，並在 `.vscode` 資料夾中新增2個檔案，分別為 `launch.json`, `tasks.json`

注意 `.vscode` 資料夾建立的位置，例如一個工作區內有數個不同的資料夾，而要運行指令的可能是其中某個資料夾內的某個資料夾，那就是把 `.vscode` 資料夾放在要運行指令的目錄中，例:

```markdown
|-- (工作區)
|   └── folder1
|   └── folder2
|   └── jekyll_project
|   |   └── .vscode
|   |   |   └── launch.json
|   |   |   └── tasks.json
|   |   └── assets
|   |       └── css/main.scss
|   |       └── js/script.js
|   └────── _config.yml
|   └── example
|       └── test
|
```

```json
# .vscode/launch.json
{
  "configurations": [
  {
    "name": "Jekyll",
    "request": "attach",
    "type": "pwa-msedge",
    "webRoot": "${workspaceFolder}",
    "preLaunchTask": "jekyll"
  }
  ]
}
```

```json
# .vscode/tasks.json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "jekyll",
      "command": "bundle exec jekyll serve",
      "type": "shell",
      "options": {
        "cwd": "${workspaceFolder}"
      }
    }
  ]
}
```

新增完成後按下 `F5` 或是從編輯器介面 `執行 >> 偵錯` 在編輯器中的整合式終端機指令將會開始運行。
