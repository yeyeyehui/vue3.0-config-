# vue3.0 config的配置和模拟接口获取数据

vue cli3.0项目中需要配置其他参数时，需要新建文件'vue.config.js'，(这文件名是固定这么写的)，与package.json在同一级目录下。

```javascript
const goods = require('./data/goods.json');  //加载json文件模拟数据
const ratings = require('./data/ratings.json');
const seller = require('./data/seller.json');

module.exports = {
  // 项目部署的基础路径
  // 我们默认假设你的应用将会部署在域名的根部，
  // 比如 https://www.my-app.com/
  // 如果你的应用时部署在一个子路径下，那么你需要在这里
  // 指定子路径。比如，如果你的应用部署在
  // https://www.**.com/my-app/
  // 那么将这个值改为 `/my-app/`
  baseUrl: '/my-app/',　　/*   默认打开的页面目录是/my-app/  */

  // 将构建好的文件输出到哪里,用于打包
  outputDir: 'dist',

  // 放置静态资源的地方 (js/css/img/font/...)
  assetsDir: '',

  // 用于多页配置，默认是 undefined
  pages: {
    index: {
      // 入口文件
      entry: 'src/main.js',　　/*这个是根入口文件*/
      // 模板文件
      template: 'public/index.html',
      // 输出文件
      filename: 'index.html',
      // 页面title
      title: 'Index Page'
    },
    // 简写格式
    // 模板文件默认是 `public/subpage.html`
    // 如果不存在，就是 `public/index.html`.
    // 输出文件默认是 `subpage.html`.
    subpage: 'src/main.js'　　　　/*注意这个是*/
  },

  // 是否在保存的时候使用 `eslint-loader` 进行检查。
  // 有效的值：`ture` | `false` | `"error"`
  // 当设置为 `"error"` 时，检查出的错误会触发编译失败。
  lintOnSave: true,

  // 使用带有浏览器内编译器的完整构建版本
  // 查阅 https://cn.vuejs.org/v2/guide/installation.html#运行时-编译器-vs-只包含运行时
  runtimeCompiler: false,

  // babel-loader 默认会跳过 node_modules 依赖。
  // 通过这个选项可以显式转译一个依赖。
  transpileDependencies: [/* string or regex */],

  // 是否为生产环境构建生成 source map？
  productionSourceMap: true,

  // 调整内部的 webpack 配置。
  // 查阅 https://github.com/vuejs/vue-docs-zh-cn/blob/master/vue-cli/webpack.md
  chainWebpack: () => {},
  configureWebpack: () => {},

  // CSS 相关选项
  css: {
    // 将组件内的 CSS 提取到一个单独的 CSS 文件 (只用在生产环境中)
    // 也可以是一个传递给 `extract-text-webpack-plugin` 的选项对象
    extract: true,

    // 是否开启 CSS source map？
    sourceMap: false,

    // 为预处理器的 loader 传递自定义选项。比如传递给
    // sass-loader 时，使用 `{ sass: { ... } }`。
    loaderOptions: {},

    // 为所有的 CSS 及其预处理文件开启 CSS Modules。
    // 这个选项不会影响 `*.vue` 文件。
    modules: false
  },

  // 在生产环境下为 Babel 和 TypeScript 使用 `thread-loader`
  // 在多核机器下会默认开启。
  parallel: require('os').cpus().length > 1,

  // PWA 插件的选项。
  // 查阅 https://github.com/vuejs/vue-docs-zh-cn/blob/master/vue-cli-plugin-pwa/README.md
  pwa: {},

  // 配置 webpack-dev-server 行为。跨域等
  devServer: {
    open: true,  //启动项目后自动打开默认浏览器
    host: 'locahost',  //打开浏览器的域名
    port: 8080,  //默认打开浏览器的端口
    https: false,  //是否开启https协议打开
    hotOnly: false,
    // 查阅 https://github.com/vuejs/vue-docs-zh-cn/blob/master/vue-cli/cli-service.md#配置代理
    proxy: {
      //跨域
      '/api': {  //拦截请求修改端口协议以及域名达到跨域
        'target': 'http://localhost:5000/api'，
        ws: true,
        changOringin: true,
        pathRewrite: {
        '^/api': 'http:localhost/8080'  //请求为api开头的替换成8080
      }
      }
    },
  before: app => {  //模拟接口，获取json数据，直接发送请求就可以获取数据
    //http://localhost:8081/api/goods
    app.get('/api/goods', (res, err) => {
      err.json(goods)
    });
    app.get('/api/ratings', (res, err) => {
      err.json(ratings)
    });
    app.get('/api/seller', (res, err) => {
      err.json(seller)
    })
  }
  },

  // 三方插件的选项
  pluginOptions: {
    // ...
  }
}
```

