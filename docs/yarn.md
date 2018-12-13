# yarn
Yarn 对你的代码来说是一个包管理器， 你可以通过它使用全世界开发者的代码， 或者分享自己的代码。
## 安装&更新
1. 安装yarn
下载地址：https://yarn.bootcss.com/docs/install/#windows-stable
安装完成后，运行如下命令，检查版本
```
yarn -v
```

## 常用命令
- 初始化
```
yarn init
yarn init --y
```

- 安装包
```
\\  安装 package.json 中的所有文件
yarn install
\\ 安装包
yarn install <packageName>
\\  添加依赖会更新 package.json 以及 yarn.lock 文件
yarn add packagename
\\ 安装全局包
yarn global add <packageName>
```

- 查看已安装的模块
```
yarn list
yarn info <packageName>
```
- 卸载已经安装的模块
```
yarn remove <packageName>
```

- 更新
```
\\ 更新全部包
yarn upgrade
\\ 更新指定包
yarn upgrade <packageName>
```
- 缓存
```
// 列出已缓存的每个包
yarn cache list
// 返回 全局缓存位置
yarn cache dir
// 清除缓存
yarn cache clean
```
- 运行脚本
用来执行在 package.json 中 scripts 属性下定义的脚本
```
yarn run <command>
// 简写的运行脚本方式
yarn start
yarn test
```