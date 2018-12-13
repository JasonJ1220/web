# NPM
npm is the world’s largest software registry. Open source developers from every continent use npm to share and borrow packages, and many organizations use npm to manage private development as well.

## 安装&更新
1. 安装 Node.js和npm
下载地址：https://nodejs.org/en/download/
安装完成后，运行如下命令，检查版本
```
node -v
```
2. 更新 npm
建议和Nodejs一起更新，重新下载Nodejs。
```
npm install npm@latest -g
```

## 常用命令
- 初始化
这个命令会在当前目录生成一个package.json文件，这个文件中会记录一些关于项目的信息，比如：项目的作者，git地址，入口文件、命令设置、项目名称和版本号等等，一般情况下这个文件是必须要有的，方便后续的项目添加和其他开发人员的使用。
```
npm init
npm init -y
```

- 安装包
```
// 在当前目录安装包
npm install <packageName>
// 指定安装版本
npm install <packageName> 0.0.1
// save、-S参数意思是把模块的版本信息保存到dependencies（生产环境依赖）中，即你的package.json文件的dependencies字段中；
npm install <packageName> --save 或 -S
// 安装全局的模块（不加参数的时候默认安装本地模块）
npm install <packageName> -g 或 --global
```

- 查看已安装的模块
```
npm list 或 npm ll 或 npm la 或 npm ls
```
- 卸载已经安装的模块
```
npm uninstall <packageName>
```

- 更新
更新已经安装的模块(或全局的模块)
```
npm update [-g]
```
- 缓存
```
// 验证缓存数据的有效性和完整性，清理垃圾数据。
npm cache verify
// 清理缓存
npm cache clean -f
```
- 运行脚本
用来执行在 package.json 中 scripts 属性下定义的脚本
```
npm run <command>
// 简写的运行脚本方式
npm start
npm test
```