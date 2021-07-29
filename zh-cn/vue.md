# vue
## 一、概述
本规范旨在为前端程序的开发者提供规范化的指导，用于程序员个人编译环境以及研发团队集成环境等场合的代码规范化检查。以求达到不管有多少人共同参与同一项目，每一行代码的编写风格尽量一致的效果

## 二、vue 项目结构

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

----
## 三、vue 代码风格规范
##### 1. vue组件命名
  > 统一使用 PascalCase (单词首字母大写命名)声明约定
  > 1. 单词大写开头对于代码编辑器的自动补全最为友好，因为这使得我们在 JS(X) 和模板中引用组件的方式尽可能的一致。
  > 2. 对于绝大多数项目来说，组件名应该总是 PascalCase 的, ——但是在 DOM 模板中总是 kebab-case 的
```vue
    // 组件名为 MyComponent 的组件在模板里使用
    <template>
      <my-component></my-component>
    </template>
```
  > 3. 页面命名统一kebab-case声明约定 形如: goods-list order-list
----
##### 2. vue页面级组件命名
  > 统一使用kebab-case，跟组件区别开来
##### 3. vue组件 props 规范
   > Prop 定义应该尽量详细

 ```vue
    // 好的做法
    <script>
      props: {
        status: String,
        default: '2'  
      }
    </script>
```   

```vue
    // 更好的做法！
    <script>
      props: {
        status: {
          type: String,
          default: '2',
          required: true, 
          validator: function (value) {
            return [
              'syncing',
              'synced',
              'version-conflict',
              'error'
            ].indexOf(value) !== -1
          }
        }
      }
    </script>
```

   > props 应该始终使用 camelCase，而在模板中应该始终使用 kebab-case

 ```vue
    <template>
      <welcome-message greeting-text="hi"></welcome-message>
    </template>
    <script>
      props: {
          greetingText: String
      }
    </script>
```
----
##### 3. vue 模板中的表达式
   > 组件模板应该只包含简单的表达式，复杂的表达式则应该重构为计算属性或方法。

```vue
  <template>
    <span>{{ normalizedFullName }}</span>
  </template>

  <script>
    // 复杂表达式已经移入一个计算属性
    computed: {
      normalizedFullName: function () {
        return this.fullName.split(' ').map(function (word) {
          return word[0].toUpperCase() + word.slice(1)
        }).join(' ')
      }
    }
  </script>
```
----
##### 4. v-for 使用
   > 总是用 key 配合 v-for
   > 1. 在组件上总是必须用 key 配合 v-for，以便维护内部组件及其子树的状态。
   > 
   > 永远不要把 v-if 和 v-for 同时用在同一个元素上
   > 1. 每次渲染都会先循环再进行条件判断 带来性能方面的浪费
   > 2. 解决方案: 
   >    * 如果条件出现在循环外部，可通过则在外层嵌套一层 DOM 标签，在这一层进行v-if判断，然后在内部进行v-for循环
   >    * 如果条件出现在循环内部，可通过计算属性computed提前过滤掉那些不需要显示的项

----

## 四、JavaScript 代码风格规范
##### 1. 对象属性的访问
>  当访问可能不存在的对象属性时 优先使用[可选链操作符( ?. )](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Optional_chaining)
> 
>  当使用可选链时返回 null或者 undefined 时,需要设置一个默认值, 优先使用[空值合并操作符（??）](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator)


   * 在 vue template 里面的用法

```javascript
<h1>可选链: {{ $$(userInfo, ['profile', 'icon'], 'defaultValue') }}</h1>
```

* 在纯 JavaScript 里面的用法


```javascript
 const icon = userInfo?.profile?.icon ?? 'defaultVlue'
```

* 在项目中支持 可选链操作符 和 空值合并操作符
> 为了避免出现兼容问题，安装并配置好对应的转换器
> 1. @babel/plugin-proposal-optional-chaining
> 2. @babel/plugin-proposal-nullish-coalescing-operator
> 3. vue 的 template 中暂时还不支持可选链语法


```javascript
//1. 定义全局函数
/**
 * 解决Vue Template模板中无法使用可选链的问题
 * @param obj
 * @param rest
 * @param defaultValue
 * @returns {*}
 */
export const optionalChaining = (obj, rest, defaultValue = '') => {
    let tmp = obj;
    for (let key in rest) {
      let name = rest[key];
      tmp = tmp?.[name] ?? defaultValue;
    }
    return tmp;
  };

// 2. 在 main.js 中全局注入optionalChaining函数
Vue.prototype.$$ = optionalChaining;
```

##### 2. 使用 ES6 风格编码
   * 定义变量使用 let ,定义常量使用 const, 不要再使用 var
   * 静态字符串一律使用单引号，动态字符串使用反引号

```javascript
   const firstName = 'Jack';
   const fullName = `${firstName} Mark`
```
   * 需要使用函数表达式的场合，尽量用箭头函数代替。因为这样更简洁，而且绑定了 this
   * 函数默认值, 函数默认值写在其他参数后面
   * 相等性判断 一律使用严格相等比较 (===)

```javascript
   function getUserInfo(name, age = 12) {
      console.log(name, age)
   }
```
   * 代码块中避免多余留白

##### 3. 命名规范
   >  一个不好的命名，可能会引起别人错误理解，对开发效率，项目的质量影响很大；从维护项目上，遵循一套合理的命名规范，无论是对自己还是接手的他人，都会大大降低代码的维护成本.
   >
   > 1. 变量
   > * 重要的变量命名需要加上备注
   > * 禁止 obj = {} , aaa = ''; b = ''; list2 = []; list4 = []; 等等这些无意义且容易让人产生疑惑的命名方式.
   > * 统一采用Camel Case 小驼峰式命名法 形如：studentInfo userInfo
   > * 实在不会的英文单词 自行使用翻译工具翻译成英文,并且加上变量备注
   > 
   [参考资料](https://segmentfault.com/a/1190000020039039)
##### 4. 三元运算符的使用
   >  不要这样写: sthTrue? true : false ? 'string' : 'number', 嵌套过深，会导致自己或其他人理解困难。
   >  可以这样写: const isTrue = a === b ? true : false;  

## 五、样式书写规范
1. 根据项目设计图原型图归纳出本项目的一些公共样式类(功能类) 这些公共样式类尽量简单化 一个样式实现一个功能 实现原子化CSS
2. 在项目编码过程中优先使用预定义好的公共样式类   
   > 优势
   > * 减少了css体积，提高了css复用
   > * 减少起名的复杂度
   > 
   > 劣势
   > * 增加了记忆成本 将css拆分为原子之后，你势必要记住一些class才能书写，你写background，也要记住开头是bg
   > * 增加了html结构的复杂性 
   
```vue
  <template>
    <div id="app" class="w-full h-full flex flex-col justify-center items-center">
  </template>
```

3. 样式类公共化大致思路
   > * 弹性布局
   > * 网格布局
   > * 盒对齐
   > * 间距
   > * 尺寸
   > * 排版
   > * 背景
   > * 边框
   > * 特效
   > * 表格
   > * 过渡和动画
   > * 转换
   > * 交互

1. [参考资料 1](https://juejin.cn/post/6918971529124872205)
2. [参考资料 2](https://juejin.cn/post/6917073600474415117)
3. [参考资料 3](https://www.tailwindcss.cn/)

## 六、注释规范
1. 公共组件使用说明以及业务场景
   
```vue
  <script>
    /**
     * avatar 头像
     * @description 本组件一般用于展示头像的地方，如个人中心，或者评论列表页的用户头像展示等场所。
     * @example <u-avatar :src="src"></u-avatar>
     * /
  </script>
```
  
2. 各组件中重要函数或者props说明
   
```vue
  <script>
    props: {
      // 头像模型，square-带圆角方形，circle-圆形
      mode: {
        type: String,
        default: 'circle'
      },
    }
  </script>
```

3. 复杂的业务逻辑处理说明
4. 特殊情况的代码处理说明,对于代码中特殊用途的变量、存在临界值、函数中使用的 hack、使用了某种算法或思路等需要进行注释描述
5. 多重 if 判断语句的代码处理说明

## 七、代码检查与格式化
>项目采用 eslint 用来进行代码的校验，能够检测出代码中的潜在问题，提高代码质量；使用 prettier作为代码格式化工具，统一的代码风格能保证代码的可读性，所以提交代码前必须先格式化。
> 
>对于 mac 或者 linux 系统提交代码前会自动运行 git hook的pre-commit 检测代码问题并且格式化代码， win10 用户如果不自动运行 githook，则需自己执行npm run lint 或者 yarn lint 检测代码问题并且格式化代码


## 八、包管理工具
> 为保证运行结果的一致性和稳定性，项目将统一采用 yarn 作为包管理工具
> 
> yarn 就是一个类似于 npm 的包管理工具，它是由 facebook 推出并开源。 与 npm 相比，yarn 有着众多的优势，主要的优势在于：速度快、离线模式、版本控制。
> 
> 详细安装方式参考官网：https://yarn.bootcss.com/docs/install/#mac-stable
 
## 九、开发工具
> 优先推荐使用[WebStorm](https://www.jetbrains.com/webstorm/)进行项目开发
   * WebStorm 统一配置如下

![webstorm 配置](https://api.suboat.com/api/yichui/filepool/download/images/bbbf9e2fef6c4d6ece5bb5946d6055a5af87052e.jpg)

## 十、git 提交规范
> 在使用Git进行代码的分布式版本控制时，规范化commit message可以帮助程序猿在多人开发协作中更好的理解他人对代码的改动信息，
> 
>避免大家按照各自的理解和习惯（甚至是随意）书写，而对他人和自己造成困惑，从而增加代码审查和纠错的时间成本。
> 
> 一个规范化的commit message，具有以下作用：
> 1. 提供更多的历史信息，方便快速浏览
> 2. 可以过滤某些commit（比如文档改动），便于快速查找信息
> 3. 可以直接从commit生成CHANGELOG.md
>  

> git commit 要求: 
> 1. 简单描述主要修改类型和内容 描述为什么修改, 做了什么样的修改.
> 
> type: commit 的类型 [必填]
> 1. feat: 新特性 新功能
> 2. fix: 修改问题
> 3. refactor: 代码重构
> 4. docs: 文档修改
> 5. style: 代码格式修改
> 6. test: 测试用例修改
> 7. chore: 其他修改, 比如构建流程, 依赖管理.
> 

## 十一、交互体验优化

1. Loading
   > 如果要加载过长时间，给用户一个反馈，能让用户知道现在系统还在运行，而不是卡主了或者是系统问题。
   > 
   > 常见情景:
   > * 页面加载时
   > * 数据加载时
   > * 下拉数据列表
   > * 详情
   > * 提交数据时
   > 
   > 解决方案:
   > 
   > 在异步请求没返回数据时,加上 loading 提示. 
   > 
   
2. 数据为空提示
   > 页面块、列表等UI展示类布局的显示 遇到返回 null undefined 空字符串 应当显示'数据为空'提示,而不是留白.
   > 

3. 重复提交
   > 在提交数据时，如果不做限制，用户可以重复提交，会导致脏数据问题。
   > 
   > 常见情景:
   > * 表单提交
   > * 点赞 收藏 按钮
   > * 上传按钮
   > 
   > 解决方案:
   > 
   > 在异步的处理时，这次接口调用未返回时就不能再次提交，加上loading效果，能让用户知道现在在提交数据中，如果接口超时再重置loading变量

4. 进度提示
   > 如上传、下载资源时，显示当前进度

5. 删除操作
   > 用户的删除操作应该有确认提示信息，询问后如果的确还是要删除，再删除

6. 输入框
   > * 根据项目要求,控制输入字数限制,不让用户无限制输入
   > * 过滤输入内容的前后空格以及用户的纯空格输入
   > * 用户输入换行符是否过滤, 视情况而定
   
 7. 可点击元素鼠标变成手型
 8. 表单
   > * 支持 Enter 按键提交表单
   > * 异步验证时显示验证中状态，比如显示loading图标
   > * 必填项验证
   > * 自定义规则验证
   > * 添加验证不通过表单项的信息提示
   > * 验证通过才可以提交表单,否则不予许提交表单

## 十二、前端技术栈
1. css预处理器: scss、less
2. 基础框架: Vue、React-Native(APP开发)
3. 组件库: 
   * PC   端: [element-ui](https://element.eleme.cn/#/zh-CN/component/installation)
   * H5移动端: [vant](https://youzan.github.io/vant/#/zh-CN/) 
   * 小程序端: [uviewui](https://www.uviewui.com/)
4. 技术框架:
   * 小程序开发: [uni-app](https://uniapp.dcloud.net.cn/README)
   * pc中台-基于: [vue-element-admin](https://panjiachen.github.io/vue-element-admin-site/zh/guide/) 二次开发
5. 图标处理:
   * iconfont + SVG（设计师无法导入到iconfont的特殊彩色icon 使用 svg）
   
## todo
- [ ] 项目初始化
- [ ] 当前用户登录后被其他用户挤下线提示
- [ ] 覆盖后端返回的报错信息
