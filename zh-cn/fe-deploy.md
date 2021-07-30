# 前端开发和部署文档
## 一、vue H5 项目
## 安装 vue 开发环境(已安装可以忽略此部分)
1. 安装node
   * 前端开发框架和环境都是需要 Node.js ，先安装node.js开发环境，vue的运行是要依赖于node来实现
   * 从node.js官网下载并安装node，Windows 安装包(.msi)
   * 检查是否安装成功: 在终端输入 node -v ，回车，查看node版本号，出现版本号则说明安装成功
   
2. 包管理工具
  * 为保证运行结果的一致性和稳定性，项目将统一采用 yarn 作为包管理工具
  * yarn 就是一个类似于 npm 的包管理工具，它是由 facebook 推出并开源。 与 npm 相比，yarn 有着众多的优势，主要的优势在于：速度快、离线模式、版本控制。
  * 详细安装方式参考官网：https://yarn.bootcss.com/docs/install/#mac-stable
  * yarn 常用命令
    * 添加包（会更新package.json 和 yarn.lock）:
    - yarn add [package]  //在当前项目中添加一个依赖包，会自动更新到package.json 和 yarn.lock文件中
    - yarn  add --dev/-D   //加到devDependencies
    - yarn remove [packageName] : 移除一个包，会自动更新package.json 和 yarn.lock
    - [详细教程](https://yarn.bootcss.com/docs/)

3. 安装 @vue/cli
  * 在终端执行 yarn global add @vue/cli
  * [参考](https://cli.vuejs.org/zh/guide/installation.html)


## vue 项目结构
```
├── babel.config.js      # babel配置
├── build                # 构建相关配置
│   └── index.js
├── jest.config.js       # 单元测试配置
├── jsconfig.json
├── online.sh            # 编译到正式服脚本[执行前要先获得许可]
├── package.json         # 项目描述文件
├── postcss.config.js    # postcss 配置
├── preview.sh           # 编译到测试服脚本
├── public               # 静态资源
│   ├── favicon.ico      # favicon图标
│   └── index.html       # html模板
├── src                  # 源代码
│   ├── App.vue
│   ├── api              # 全局 api 请求
│   ├── assets           # 主题 字体 图片等静态资源
│   ├── components       # 全局公用组件
│   ├── directive        # 全局指令
│   ├── filters          # 全局 filter
│   ├── graphql          # 统一放置 graphql 查询语句
│   ├── icons            # 全局svg图标
│   ├── layout           # 全局 layout
│   ├── main.js          # 入口文件 加载组件 全局指令 filter 初始化等
│   ├── mixins           # 全局 mixins
│   ├── permission.js    # 路由权限管理
│   ├── router           # 全局路由配置
│   ├── settings.js
│   ├── store            # 全局vuex 状态管理配置
│   ├── styles           # 全局样式
│   ├── utils            # 全局公共变量 公共函数 初始化函数 websocket 初始化等配置
│   └── views            # 统一放置页面文件
├── tests                # 测试代码文件夹
│   └── unit
├── vue.config.js        # vue-cli 配置
├── .env.development     # 开发环境变量配置
├── .env.production      # 生产环境变量配置
├── .env.staging         # 测试环境变量配置
├── .eslintignore        # eslint 忽略项目
├── .eslintrc.js         # eslint 配置
└── yarn.lock            # 准确的存储每个依赖的具体版本信息，以保证在不同机器安装可以得到相同的结果。
```

## 本地运行 vue 项目
  * 安装依赖,在终端执行 yarn
  * 在终端执行 yarn dev
  * 终端输出如下本地开发调试地址,在浏览器访问以下地址即可访问

```
  App running at:
  - Local:   http://localhost:8893 
  - Network: http://192.168.6.77:8893
```
## 部署 vue 项目
#### 1. 部署到测试服
  * 在终端执行脚本 preview.sh
#### 2. 部署到正式服
  * 在终端执行脚本 online.sh
----
## 二、微信小程序(使用uni-app 开发) 项目
## 安装 vue 开发环境(已安装可以忽略此部分)
  * [uni-app 知识参考](https://uniapp.dcloud.io/)
## uni-app 项目结构
```
│─components            符合vue组件规范的uni-app组件目录
│  └─comp-a.vue         可复用的a组件
├─hybrid                App端存放本地html文件的目录
├─platforms             存放各平台专用页面的目录
├─packageX              业务分包文件存放的目录
├─pages                 业务页面文件存放的目录
├─graphql               统一放置 graphql 查询语句
├─static                存放应用引用的本地静态资源（如图片、视频等）的目录，注意：静态资源只能存放于此
├─uni_modules           存放[uni_module](/uni_modules)规范的插件。
├─wxcomponents          存放小程序组件的目录
├─main.js               Vue初始化入口文件
├─App.vue               应用配置，用来配置App全局样式以及监听 应用生命周期
├─manifest.json         配置应用名称、appid、logo、版本等打包信息
└─pages.json            配置页面路由、导航条、选项卡等页面类信息
```
## 本地运行 uni-app 项目
  * 安装依赖,在终端执行: yarn
  * 在终端执行: yarn dev
  * 用微信开发者工具打开 qzpm/dist/dev/mp-weixin 即可调式开发

## 部署 uni-app 项目
### 1. 部署上传测试版小程序
  * 在终端执行: yarn build:mp-weixin:staging
  * 用微信开发者工具打开 qzpm/dist/build/mp-weixin 检查是否是测试服接口数据,确认无误后,即可上传测试版小程序

### 2. 部署上传正式版小程序
  * 在终端执行: yarn build:mp-weixin
  * 用微信开发者工具打开 qzpm/dist/build/mp-weixin, 检查是否是正式服接口数据,确认无误后,即可上传正式版小程序

## 三、项目里的 graphql 文件夹说明
  1. graphql 文件夹统一放置 graphql 查询语句 [参考网址](https://graphql.bootcss.com/)
  2. 项目里的 graphql 语法提示说明:
    * 项目里的 graphql 语法提示基于 webstorm 的 graphql 插件(https://plugins.jetbrains.com/plugin/8097-js-graphql),
    * 在终端执行: yarn sync-schema
    * 在 graphql 文件夹里的 *-gql.js 写 graph 语句即可获得代码提示,提高开发效率和编码体验

## FAQ (常见问题)
1. 报错 permission denied: ./preview.sh
 * 说明: 这是权限问题 请执行以下命令
 * chmod  +x ./preview.sh

