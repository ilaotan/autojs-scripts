# 个人 `Auto.js` 脚本1

## 下载

### 使用构建好的 apk

1. 进入[`Actions`](https://github.com/demoshang/autojs-scripts/actions), 选择最新的构建
2. 在 `Artifacts` 中 找到 `debug-apk` 并下载
3. 解压下载的 `debug-apk`, 选择对应 apk (大部分手机使用 `inrt-armeabi-v7a-debug.apk `), 在手机中安装
4. 打开安装的软件, 按操作执行

### 已经安装 `autojs 4.1.1 Alpha2`

1. 进入[`Actions`](https://github.com/demoshang/autojs-scripts/actions), 选择最新的构建
2. 在 `Artifacts` 中 找到 `js-dist` 并下载
3. 解压下载的 `js-dist` 放置到手机中
4. 用 `autojs` 打开这个目录, 运行后按操作提示执行

## 使用

1. 音量键上键, 按一次表示暂停执行(当前逻辑要处理完), 间隔 1 秒后, 再按一次表示继续执行, 连续按 2 次以上(1 秒内), 退出本次执行
2. 提示的权限全部需要给
3. 请尽量保持网络顺畅, 防止因为网络原因导致脚本执行的延迟不匹配

## 开发说明

1. 支持 `typescript` 开发
2. 由于 ts 不支持 `Auto.js` 中 `用户界面(UI)` 相关函数(如 `ui.layout(xml)`), 需要特殊转换  
   2.1 将 `xml` 独立成文件, 如 `layout.xml`  
   2.2 在 `xml` 文件 头部添加注释 `<!-- wrap: 调用方法 -->`, 如:

   ```xml
   <!-- wrap: ui.layout -->
   <!-- 正常 XML -->

   <frame>
   <img id="img0" radius="{{__ARG0__.radius}}" w="{{__ARG0__.width}}" h="{{__ARG0__.height}}" tint="{{__ARG0__.color}}" src="data:image/gif;base64,R0lGODlhAQABAIAAAAUEBAAAACwAAAAAAQABAAACAkQBADs=" />
   </frame>
   ```

   2.3 `xml` 文件中可以使用变量 `__ARGS__`, `__ARG0__` ... `__ARG9__`

   2.4 在 `.ts` 文件中使用

   ```ts
   import layout from "./layout.xml";

   const params = {
     radius: 10,
     width: 10,
     height: 10,
     color: "purple",
   };

   // 这里的 params 在 xml 中为 __ARG0__
   // 也可以从 __ARGS__[0] 获取
   // 即:
   // 函数的 arguments 在 xml 中为 __ARGS__
   // 函数的 第1个参数  在 xml 中为 __ARG0__
   // 函数的 第9个参数  在 xml 中为 __ARG9__
   layout(params);
   ```

3. `projects/common/**/*` 为公用函数
4. `projects/**/project.json` 下和 `project.json` 同级的表示一个 `project`, 必须存在 `main.ts`, 即对于 `Auto.js` 来说, 入口必须是 `main.ts(main.js)`, [`project.json` 格式如下](https://github.com/demoshang/autojs-replace-inrt#%E4%BD%BF%E7%94%A8)

   ```js
   {
     // app 的名字
     "name": "xxx",
     // 包的名字, 符合android规定
     "packageName": "com.autojs.demo.xxx",
     // app 的 logo, 必须是 png 图片
     "logo": "xxx.png",
     // app 启动页面图片, 必须是 png 图片
     "splash": "xxx.png",
     // 入口文件必须是 main.js
     "main": "main.js",
     // 版本号
     "versionName": "0.1.0",
     // 版本号的数字表示
     "versionCode": 700,
     "ignore": []
   }
   ```

## 本地开发

1. `clone`
2. `npm install`
3. `npm start`
4. `dist` 为生成目录, 可用于 `Auto.js` 运行

## 开发依赖

1. [`Visual Studio Code`](https://code.visualstudio.com/)
2. [`Auto.js-VSCode-Extension`](https://github.com/demoshang/Auto.js-VSCode-Extension/releases/tag/v1.0.0) Visual Studio Code 支持 Auto.js 开发, 修改原版使其支持 `dist` 目录

## 打包

- 构建成单独 `App` 请看 [.github/workflows/build.yml](./.github/workflows/build.yml)

## Thanks

1. [Auto.js](https://github.com/hyb1996/Auto.js)
2. [autojs-dev](https://github.com/pboymt/autojs-dev)
