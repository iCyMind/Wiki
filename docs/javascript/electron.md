# Electron

## 获取appdata路径

```js
const { dialog, app, getCurrentWindow  } = require('electron').remote
const dbfile = path.join(app.getPath('appData'), 'Tunnel-Inspector/CMCC.db')
```

```js
process.env.APPDATA || (process.platform == 'darwin' ? path.join(process.env.HOME, 'Library', 'Application Support') : '/var/local')
```

## 用默认浏览器打开url

[link](https://github.com/electron/electron/issues/1344)

```js
var handleRedirect = (e, url) => {
  if(url != webContents.getURL()) {
    e.preventDefault()
    require('electron').shell.openExternal(url)
  }
}

webContents.on('will-navigate', handleRedirect)
webContents.on('new-window', handleRedirect)
```

用了vue之后以上方法貌似不太管用, 用以下的更保险:

```js
const shell = require('electron').shell
document.on('click', 'a[href^="http"]', function (event) {
  event.preventDefault()
  shell.openExternal(this.href)
})
```
