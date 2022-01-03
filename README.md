## Nuxt搭配pm2實現電腦重開機時能自動重啟


### 一、下載PM2

參考：https://www.npmjs.com/package/pm2

### 二、程式根目錄中加入pm2.start.json

```json
{
    "apps": {
        "name": "ProjectName",
        "exec_mode": "cluster",
		"script": "./node_modules/nuxt/bin/nuxt.js",
		"args": "start",
        "watch": true,
        "ignore_watch": [
            "node_modules",
            "Logmanager"
        ],
        "error_file": "Logmanager/pm2/err.log",
        "out_file": "Logmanager/pm2/out.log",
        "log_date_format": "YYYY-MM-DD HH:mm:ss"
    }
}
```

### 三、package.json加入scripts(pm2-prod、pm2-prod-restart)

- pm2-prod：表示用pm2啟動程式
- pm2-prod-restart：表示用pm2重啟程式

```
{
  ...,
  
  "scripts": {
    "pm2-prod": "pm2 start pm2.start.json",
    "pm2-prod-restart": "pm2 restart pm2.start.json"
  }
}
```

### 四、加入ReStart.bat檔

```
cd 專案目錄
npm run pm2-prod
```

### 五、新增「電腦重啟時要執行的batch檔」(call.bat)

- 若有多個要重啟的batch已”&”分隔開來

```
"專案路徑\ReStart.bat" & "…"
```

### 六、設定電腦重啟執行程式

- 方法一、須登入才會執行指令

    將call.bat放至C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup

- 方法二、不須登入就會執行指令

    將call.bat新增到工作排程器中

    1. 設定執行帳號
    
        ![image](https://github.com/kuo0422/NuxtAddPm2/blob/main/Images/%E8%A8%AD%E5%AE%9A%E5%80%BC%E8%A1%8C%E5%B8%B3%E8%99%9F.png)

    2. 設定觸發程序為啟動時
    
        ![image](https://github.com/kuo0422/NuxtAddPm2/blob/main/Images/%E8%A7%B8%E7%99%BC%E7%A8%8B%E5%BA%8F%E8%A8%AD%E5%AE%9A%E7%82%BA%E5%95%9F%E5%8B%95%E6%99%82.png)

    3. 設定啟動程式，路徑為步驟五的batch檔
    
        ![image](https://github.com/kuo0422/NuxtAddPm2/blob/main/Images/%E5%95%9F%E5%8B%95%E7%A8%8B%E5%BC%8F.png)

    4. 條件設定
    
        ![image](https://github.com/kuo0422/NuxtAddPm2/blob/main/Images/%E6%A2%9D%E4%BB%B6%E8%A8%AD%E5%AE%9A.png)


### 七、相關參考資料

1. [pm2的npm網站]
    
    https://www.npmjs.com/package/pm2

2. [開機登入啟動bat]
    
    https://www.itread01.com/content/1548400504.html

3. [使用PM2部署Nuxt]
    
    https://nuxtjs.org/docs/2.x/deployment/deployment-pm2
    
