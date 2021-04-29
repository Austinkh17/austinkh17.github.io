---
title: electron将vue打包成exe初体验
date: 2021-04-25 18:18:05
tags: 
- electron 
- vue
categories: electron
---

> Electron是一个基于Chromium和 Node.js，可以使用 HTML、CSS和JavaScript构建跨平台应用的技术框架，兼容 Mac、Windows 和 Linux。虽然B/S是目前开发的主流，但是C/S仍然有很大的市场需求。受限于浏览器的沙盒限制，网页应用无法满足某些场景下的使用需求，而桌面应用可以读写本地文件、调用更多系统资源，再加上Web开发的低成本、高效率的优势，这种方式越来越受到开发者的喜爱。

【注】`SimulatedGREG/electron-vue`已经很久没有更新了，而且其生成的工程结构并不是vue-cli3。所以不推荐使用。

<!--more-->

### 基于已有的vue项目融入electron来搭建桌面应用

#### 手动安装Electron相关依赖
修改package.json,添加以下7行,并执行`cnpm i`：
```
"scripts": {
  "serve": "vue-cli-service serve",
  "build": "vue-cli-service build",
  "lint": "vue-cli-service lint",
  "electron:build": "vue-cli-service electron:build", // add
  "electron:serve": "vue-cli-service electron:serve", // add
  "postinstall": "electron-builder install-app-deps", // add
  "postuninstall": "electron-builder install-app-deps" // add
},
"main": "background.js",      // add
"devDependencies": {
  "electron": "^12.0.2",       // add
  "electron-builder": "^22.10.5",       // add
  "vue-cli-plugin-electron-builder": "~2.0.0-rc.6",       // add
},
```

#### 新建src/background.js
``` 
'use strict'

import { app, protocol, BrowserWindow } from 'electron'
import { createProtocol } from 'vue-cli-plugin-electron-builder/lib'
// import installExtension, { VUEJS_DEVTOOLS } from 'electron-devtools-installer'
const isDevelopment = process.env.NODE_ENV !== 'production'

// Keep a global reference of the window object, if you don't, the window will
// be closed automatically when the JavaScript object is garbage collected.
let win

// Scheme must be registered before the app is ready
protocol.registerSchemesAsPrivileged([
  { scheme: 'app', privileges: { secure: true, standard: true } }
])

function createWindow() {
  // Create the browser window.
  win = new BrowserWindow({
    width: 1000,
    height: 600,
    webPreferences: {
      // Use pluginOptions.nodeIntegration, leave this alone
      // See nklayman.github.io/vue-cli-plugin-electron-builder/guide/security.html#node-integration
      // for more info
      // nodeIntegration: process.env.ELECTRON_NODE_INTEGRATION
      webSecurity: false,
      nodeIntegration: true
    }
  })

  if (process.env.WEBPACK_DEV_SERVER_URL) {
    // Load the url of the dev server if in development mode
    win.loadURL(process.env.WEBPACK_DEV_SERVER_URL)
    if (!process.env.IS_TEST) win.webContents.openDevTools()
  } else {
    createProtocol('app')
    // Load the index.html when not in development
    win.loadURL('app://./index.html')
  }

  win.on('closed', () => {
    win = null
  })
}

// Quit when all windows are closed.
app.on('window-all-closed', () => {
  // On macOS it is common for applications and their menu bar
  // to stay active until the user quits explicitly with Cmd + Q
  if (process.platform !== 'darwin') {
    app.quit()
  }
})

app.on('activate', () => {
  // On macOS it's common to re-create a window in the app when the
  // dock icon is clicked and there are no other windows open.
  if (win === null) {
    createWindow()
  }
})

// This method will be called when Electron has finished
// initialization and is ready to create browser windows.
// Some APIs can only be used after this event occurs.
app.on('ready', async () => {
//   if (isDevelopment && !process.env.IS_TEST) {
//     // Install Vue Devtools
//     try {
//       await installExtension(VUEJS_DEVTOOLS)
//     } catch (e) {
//       console.error('Vue Devtools failed to install:', e.toString())
//     }
//   }
  createWindow()
})

// Exit cleanly on request from parent process in development mode.
if (isDevelopment) {
  if (process.platform === 'win32') {
    process.on('message', (data) => {
      if (data === 'graceful-exit') {
        app.quit()
      }
    })
  } else {
    process.on('SIGTERM', () => {
      app.quit()
    })
  }
}
```

#### vue.config.js添加打包配置
```
pluginOptions: { // 第三方插件配置
    electronBuilder: {
        builderOptions: {
            appId: "cn.kk",
            productName: "k",
            icon: "./public/k.ico",
            directories: {
                output: "./dist_electron"//输出文件路径
            },
            win: {
                icon: "./public/k.ico",
                target: ["zip", "nsis"]
            },
            nsis: {
                oneClick: false,
                allowElevation: true,
                allowToChangeInstallationDirectory: true,
                installerIcon: "./public/k.ico",
                uninstallerIcon: "./public/k.ico",
                installerHeaderIcon: "./public/k.ico",
                createDesktopShortcut: true,
                createStartMenuShortcut: true
                // license: "./LICENSE.txt"
            }
        }
    }
}
```

#### 编译并启动APP
执行以下命令，开始编译APP，并启动开发环境APP：
$ npm run electron:serve

首次启动可能会等待很久，加载完后会自动打开APP，等待即可。

编译成功后，就会出现开发环境的APP了

当回到控制台，发现控制台的信息：
```
INFO  Launching Electron...
Failed to fetch extension, trying 4 more times
Failed to fetch extension, trying 3 more times
Failed to fetch extension, trying 2 more times
Failed to fetch extension, trying 1 more times
Failed to fetch extension, trying 0 more times
Vue Devtools failed to install: Error: net::ERR_CONNECTION_TIMED_OUT
```
这是因为在请求安装vuejs devtools插件。需要***才能安装成功。可以忽略上面的问题，耐心等待5次请求失败后会自动跳过，上面的成功界面即证实跳过依然编译成功。

依然有解决方案：

注释掉src/background.js中的以下代码就行了：

/* if (isDevelopment && !process.env.IS_TEST) { // Install Vue Devtools try { await installVueDevtools(); } catch (e) { console.error("Vue Devtools failed to install:", e.toString()); } } */

###  报错排查
#### 1.使用electron-builder打包成安装包错误，下载不来winCodeSign

electron-builder有一些依赖包如 【winCodeSign】、【nsis】、【nsis-resources】；在科学上网的情况下，直接执行命令就会自动处理相关依赖，但天朝基本不科学上网，所以会出现包安装超时等的一些情况，这个时候就需要手动安装：

1.第一步：下载wincodesign、nsis、nsis-resources对应版本的包 链接: https://github.com/electron-userland/electron-builder-binaries/releases

2.第二步：选择7z下载并解压

3.第三步：在%LOCALAPPDATA%（C:\Users\Administrator\AppData\Local）目录下新建electron-builder\cache\

4.第四步：将解压的文件放到第三步的目录下，根据错误提示的版本在对于的包下面新建对于的版本，并把文件移动到目录下，便可正常打包，路径如下：
cache\winCodeSign\winCodeSign-2.6.0
cache\nsis\nsis-3.0.4.1
cache\nsis\nsis-resources-3.4.1

各系统缓存地址：
macOS: ~/Library/Caches/electron-builder
Linux: ~/.cache/electron-builder
windows: %LOCALAPPDATA%\electron-builder\cache

#### ico需要256x256大小的，推荐转换地址(https://convert.72wo.com/png-to-icns)
