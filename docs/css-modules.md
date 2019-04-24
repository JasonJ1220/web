# css modules: 前端模块化
在 css 模块化的解决方案中， 存在两种解决方案， 一种是放弃使用 css, 使用 js 或者 json 的形式来重写 css, 这样写的好处在于方便利用 js 的模块化管理，缺点在于缺少 css 预处理器例如 less sass 等的支持，例如 react-style 属于这种解决方案。另外一种是依旧采用 css 的原生态写法， 但是通过 js 来进行管理依赖， 下面的 css-modules 属于这种的写法。

##　css 模块化中的一些问题
在 css 的模块化的过程中， 下面几种问题是需要解决的：

### 全局污染
在 css 中， 样式 style 是属于全局范围内的， 因此当我们在全局范围内w为某一元素标签添加样式的时候， 这一样式会被应用到全局的所有的该标签下的元素上面，为了减少掉全局变量的污染，我们会采用提高元素优先级的方法进行样式覆盖， 例如添加 !important ，获取通过复杂选择器增大元素的权重值以及行内样式，这种通过提高元素优先级的方法实现的元素样式覆盖导致了无法重写元素样式， 丧失了灵活性。

### 命名混乱
混乱的命名方式会导致开发过程中的样式冲突， 样式混乱， 为后续开发带来了困难， 在这个问题上面， 有一种解决方式， 就是采用 BEM 的命名方式；

### 依赖管理不彻底
在引入组件的时候， 组件应该相互独立， css 文件应该随着组件的引入按需加载，而不是引入所有的样式， 这样造成了模块的浪费。

### 实现变量共享
在复杂的组件中, 可能需要通过 js 或者 css 同时进行操作样式，这种情况下可能会造成样式的冗余，现在的预处理器都不能实现变量在 css 与 js 之间的变量共享。

## CSS modules
css modules 是一种模块化的解决方案，通过使用 js 文件引入管理， 同时， 在 css modules 中的 css 文件中的样式名被默认为局部样式，从而避免了局部样式的污染， 结合 webpack, 使用 css modules 应用的类名会被编译为一串字符， 从而避免 class 命名的重复。

下面是一些基本的语法:
### 基本语法
#### 引入与导出
在需要使用 css 的组件中通过使用 import 的方法进行导入：

```
// index.css:
.btn { /* btn 的相关样式 */ }

// index.js:

import style from "./index.css";
// ...
render () {
    return <button className={ style.btn }></button>
}
// ...
```

最终生成：
```
<button class="btn"></button>
```

通过使用 js 引入的方式还可以实现常量的共享功能：
对于常量， 需要使用 :export关键字将 css 中的常量输出到 js 中:
```
@color: "red";

:export {
    color: @color;
}

import style from "./index.css";

style.color // "red"

```

#### 全局与局部样式
通过使用 :global 和 :local 样式可以将当前的样式声明为全局和局部样式：
```
.btn {
    color: blue;
}

// 等同于下面的 local

:local(.btn) {
    color: blue;
}

// 声明为全局样式
// 文件中所有类名为 `btn` 的元素都将应用这个样式
:global {
    .btn {
        color: blue;
    }
}
```

#### 代码复用与样式组合
在 css-modules 中使用 composes 来实现多个类名的组合， 例如， 当我们定义组件 Button 的时候， 我们需要定义基础配置， 大小以及类型样式， 可以这么写
```
.btn {
    // 定义基础样式
}
.btn-small {
    // small 大小相关样式
}
.btn-text {
    // text btn 相关样式
}
<!-- 组合样式  -->
.btn-small-text {
    composes: btn btn-small btn-text;
}

// 或者当 btn 为全局样式 global-btn 的时候， 可以进行组合

.btn-small-text {
    composes: global-btn btn-small btn-text;
}
```

注意：**使用 composes进行样式组合的时候， 需保证组合的样式名在composes之前已经存在了， 并且是单独的样式名， 没有嵌套关系**

## 使用技巧
- **减少 class 嵌套, 使用单独 class 来定义样式，减少 class 层叠**
- **使用 composes 类名组合来组合样式**
- **不使用选择器， 仅仅使用 class 来定义样式**

##　CSS modules 使用
项目中使用 less 作为 css 预处理器，在 webpack 中 css-loader 支持 css modules, 相关配置如下:
```
test: /\.less$/,
use: [
    {
    loader: 'style-loader'
    },
    {
    loader: 'css-loader',
    options: {
        // 启用 css modules
        modules: true,
        // 定义最终编译之后的样式名称
        // local: 应用的 class 名称
        // hash: 编译时随机生成的 hash 值， 避免 class 名称重复
        localIdentName: '[local]--[hash:base64:5]'
    }
    }, 
    {
        loader: 'less-loader'
    }
]
```






