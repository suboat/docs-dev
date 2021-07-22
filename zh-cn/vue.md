# vue
## 一、概述
本规范旨在为前端程序的开发者提供规范化的指导，用于程序员个人编译环境以及研发团队集成环境等场合的代码规范化检查。以求达到不管有多少人共同参与同一项目，每一行代码的编写风格尽量一致的效果

## 二、vue 代码风格规范
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
## 三、JavaScript 代码风格规范
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


## 四、样式书写规范
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

## 五、注释规范
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

## 六、代码检查与格式化
>项目采用 eslint 用来进行代码的校验，能够检测出代码中的潜在问题，提高代码质量；使用 prettier作为代码格式化工具，统一的代码风格能保证代码的可读性，所以提交代码前必须先格式化。
> 
>对于 mac 或者 linux 系统提交代码前会自动运行 git hook的pre-commit 检测代码问题并且格式化代码， win10 用户如果不自动运行 githook，则需自己执行npm run lint 或者 yarn lint 检测代码问题并且格式化代码


## 七、包管理工具
> 为保证运行结果的一致性和稳定性，项目将统一采用 yarn 作为包管理工具
> 
> yarn 就是一个类似于 npm 的包管理工具，它是由 facebook 推出并开源。 与 npm 相比，yarn 有着众多的优势，主要的优势在于：速度快、离线模式、版本控制。
> 
> 详细安装方式参考官网：https://yarn.bootcss.com/docs/install/#mac-stable
 
## 七、开发工具
> 优先推荐使用[WebStorm](https://www.jetbrains.com/webstorm/)进行项目开发
   * WebStorm 统一配置如下

![webstorm 配置](https://api.suboat.com/api/yichui/filepool/download/images/bbbf9e2fef6c4d6ece5bb5946d6055a5af87052e.jpg)

## TODO
- [ ] 按钮的显示状态: 正常 禁用 加载中 
- [ ] 列表的显示状态: 加载中 有数据 无数据
