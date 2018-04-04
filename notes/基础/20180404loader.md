loader用于对模块的源代码进行转换。loader可以使你在import或加载模块的时候预处理文件

因此loader类似于其他构建工具中的task，并提供了处理前端构建步骤的强大方法

loader可以将文件从不通的语言转换为js，或者将内联图像转换为data URL。

loader甚至允许你在js模块中import CSS 文件

## 示例

使用loader首先需要安装对应的处理包，例如：

    npm install --save-dev css-loader
    npm install --save-dev ts-loader
    
然后再配置文件中
    
```javascript
module.exports = {
  module: {
    rules: [
      { test: /\.css$/, use: 'css-loader' },
      { test: /\.ts$/, use: 'ts-loader' }
    ]
  }
};
```

## 使用

有三种使用loader的方式

+ 配置（推荐）：在webpack.config.js文件中制定loader
+ 内联：在每个import语句中显示指定loader
+ CLI：在shell命令中指定

#### 配置

module.rules 允许你在 webpack 配置中指定多个 loader。 这是展示 loader 的一种简明方式，并且有助于使代码变得简洁。同时让你对各个 loader 有个全局概览：

```javascript
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: 'style-loader' },
          {
            loader: 'css-loader',
            options: {
              modules: true
            }
          }
        ]
      }
    ]
  }
```

#### 内联

可以在 import 语句或任何等效于 "import" 的方式中指定 loader。使用 ! 将资源中的 loader 分开。分开的每个部分都相对于当前目录解析。

```javascript
import Styles from 'style-loader!css-loader?modules!./styles.css';
```

选项可以传递查询参数，例如 ?key=value&foo=bar，或者一个 JSON 对象，例如 ?{"key":"value","foo":"bar"}。

#### CLI

    webpack --module-bind jade-loader --module-bind 'css=style-loader!css-loader'
    
这会对 .jade 文件使用 jade-loader，对 .css 文件使用 style-loader 和 css-loader。

## loader特性

+ loader支持链式传递，能够对资源使用流水线。一组链式的loader将按照相反的顺序执行。loader链式中的第一个loader返回值给下一个loader。再最后一个loader返回webpack所预期的js
+ loader可以是同步的，也可以是异步的
+ loader运行再Node.js中，并且能够执行任何可能的操作
+ loader接受查询参数，用于对loader传递配置
+ loader也能够使用options对象进行配置
+ 除了使用package.json常见的main属性，还可将普通的npm模块导出为loader，做法是再package.json里定义一个loader字段
+ 插件可以为loader带来更多的特性
+ loader能产生额外的任意文件

loader 通过（loader）预处理函数，为 JavaScript 生态系统提供了更多能力。 用户现在可以更加灵活地引入细粒度逻辑，例如压缩、打包、语言翻译和其他更多。

[更多loader](https://doc.webpack-china.org/loaders)

## 解析loader

loader 遵循标准的[模块解析](https://doc.webpack-china.org/concepts/module-resolution/)。多数情况下，loader 将从模块路径（通常将模块路径认为是 npm install, node_modules）解析。

loader 模块需要导出为一个函数，并且使用 Node.js 兼容的 JavaScript 编写。通常使用 npm 进行管理，但是也可以将自定义 loader 作为应用程序中的文件。按照约定，loader 通常被命名为 xxx-loader（例如 json-loader）。有关详细信息，请查看如何编写 loader？。