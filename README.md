# 库文件让他人引用的demo（简单版，不包括按需引用、tree shaking等）
假设该文件是一个库文件，如何暴露让他人引用

### 他人引用的方式有以下几种

###### es6模块或者commonjs或第三种
import library from 'library'

const library = require('library')

require(['library'], function () { })

以上三种需在webpack.config.js中加入，使库能支持上三种引入方式。
```
output: {
    libraryTarget: 'umd'
}
```
注意：若值是this，即挂载到环境的this上，若是window，则挂载在window。node可以设为global
###### script标签引入
<script src='library.js'></script> 
```
output: {
    // 全局变量名称为library
    library:'library', 
}
```

### 情况说明
1.我们的库使用了lodash，打包出的文件包含了lodash。假如A用我们的库，其业务代码中也需要lodash，他自己引用了一遍，那么出现了重复打包的情况

解决方法：在webpack.config.js中加入，意思是如果看到lodash就不打包，这样打包出的文件不含lodash
```
    module.exports = {
        externals: ["lodash"]
    }
```
https://www.webpackjs.com/configuration/externals/#externals
```
    module.exports = {
        externals: {
            lodash:{
                root:'_',// 别人通过script引入的时候，直接在挂载到全局变量，_就可以直接使用
                commonjs:'lodash'
            }
        }
    }
```

### 发布到npmjs上
npm addUser
npm login
npm publish
npm unpublish 包名

设回原仓库，不要在淘宝源上
```
npm config set registry=http://registry.npmjs.org
npm config set registry https://registry.npm.taobao.org
```