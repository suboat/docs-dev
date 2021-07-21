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
##### 2. vue组件 props 规范
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
## 三、JavaScript 书写规范
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

## 四、样式 书写规范

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


# 状态处理
1. 按钮的显示状态: 正常 禁用 加载中 
2. 列表的显示状态: 加载中 有数据 无数据
