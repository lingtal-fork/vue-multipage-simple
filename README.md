# vue-multipage-simple

> 一个vue的简单多页配置

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# run unit tests
npm run unit

# run e2e tests
npm run e2e

# run all tests
npm test
```

## 改动文件细节

### 配置改动
#### webpack.base.conf.js
有多少新增页面就应该在entry增加多少个入口的chunk
比如现在新增的admin的chunk
```js
entry: {
  app: './src/main.js',
  admin: './src/admin.js'
}
```

#### webpack.dev.conf.js
通过``html-webpack-plugin``这个plugin导出新增的页面
```js
new HtmlWebpackPlugin({
  filename: 'admin.html',
  template: 'admin.html',
  chunks: ['admin'],
  inject: true
})
```
完整配置列表：
> title : 用于生成的HTML文件的标题。
>
> filename : 用于生成的HTML文件的名称，默认是index.html。你可以在这里指定子目录（例如:assets/admin.html）
>
> template : 模板的路径。支持加载器，例如 html!./index.html。
>
> inject :true | ‘head’ | ‘body’ | false 。把所有产出文件注入到给定的 template 或templateContent。当传入 true或者 ‘body’时所有javascript资源将被放置在body元素的底部，“head”则会放在head元素内。
>
> favicon : 给定的图标路径，可将其添加到输出html中。
>
> minify : {…} | false 。传一个html-minifier 配置object来压缩输出。
>
> hash : true | false。如果是true，会给所有包含的script和css添加一个唯一的webpack编译hash值。这对于缓存清除非常有用。
>
> cache : true | false 。如果传入true（默认），只有在文件变化时才 发送（emit）文件。
>
> showErrors : true | false 。如果传入true（默认），错误信息将写入html页面。
>
> chunks : 只允许你添加chunks 。（例如：只有单元测试块 ）
>
> chunksSortMode : 在chunk被插入到html之前，你可以控制它们的排序。允许的值 ‘none’ | ‘auto’ | ‘dependency’ | {function} 默认为‘auto’.
>
> excludeChunks : 允许你跳过一些chunks（例如，不要单元测试的 chunk）.
>
> xhtml : 用于生成的HTML文件的标题。
>
> title : true | false。如果是true，把link标签渲染为自闭合标签，XHTML要这么干的。默认false。

#### webpack.prod.conf.js
同样通过``html.webpack.plugin``来新增页面
```js
new HtmlWebpackPlugin({
  filename: process.env.NODE_ENV === 'testing'
    ? 'admin.html'
    : config.build.admin,
  template: 'admin.html',
  inject: true,
  minify: {
    removeComments: true,
    collapseWhitespace: true,
    removeAttributeQuotes: true
    // more options:
    // https://github.com/kangax/html-minifier#options-quick-reference
  },
  // necessary to consistently work with multiple chunks via CommonsChunkPlugin
  chunksSortMode: 'dependency'
}),
```
### 新增文件
在根目录新增``xxx.html``文件，在src目录新增``xxx.js``入口文件。
`` webpack.base.conf.js``配置的entry是指向的``xxx.js``文件路径。
``HtmlWebpackPlugin``的template是指向的``xxx.html``文件路径。

## 注意
如果要使用多页配置，就不能使用``vue-router``。直接通过``http://xxx.com/xx.html``来访问页面，而不再使用``vue-router``来触发不同组件。
