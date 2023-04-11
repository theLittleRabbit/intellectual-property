# intellectual-property
legal affairs-intellectual property
# 知识产权管理PC端

## Project setup

```
yarn install
```

### Compiles and hot-reloads for development

```
yarn serve
```

### Compiles and minifies for production

```
yarn build
```

### Lints and fixes files

```
yarn lint
```

### 目录结构

+ public - 静态文件，不会被转译
    + index.html - 网站访问时的主html文件，主题色声明与切换的逻辑，现在写在了这个文件中
    + element-ui-style - 存放element UI不同主题色的css文件，方便动态切换，现有红色与蓝色两种
+ src - 主代码目录
    + assets - 静态文件，会被打包转译
        + css - 存放单独css文件的目录
            + element-variables.scss - 重写element UI部分样式文件，使其符合方圆合规组件样式规范
            + functions.scss - 存放sccs声明的公共函数（该文件会经过webpack loader引入到每个vue的style中，使用时不需要单独引入）
            + mixins.scss - 初始化css配置，以及通用的css class（该文件会在app.vue文件中引入，使用时直接在标签上设置class）
            + variables.scss - 存放sccs声明公共变量（该文件会经过webpack loader引入到每个vue的style中，使用时不需要单独引入）
        + images - 存放图片资源的目录(不随主题色改变)
            + 如果项目图片比较多，可在按页面结构划分子文件夹（默认使用扁平化方式存放）
        + iconSvg - 跟随主题色改变的svg图标（该文件夹下的svg文件经过svg-sprite-loader转化，使用<svg-icon icon-class="xxx"></svg-icon> xxx 为 svg 的名字 来显示图标）
            + svg文件，通过修改`fill="var(--primary-color)"`改变图标颜色，svg组件名就是该文件名（禁止使用下划线）
    + components - 公共组件的目录
    + config - 管理配置的目录
    + lib - 管理基础建设的目录
        + directive - 存放自定义指令的目录
        + Create.js - 动态创建组件的方法
        + eventBus.js - 事件总线，用于复杂场景的事件通讯
        + loadingManage.js - loading展示与隐藏逻辑管理，用在request.js中用来统一控制全局loading的展示，减少在单独请求中重复管理loading的逻辑
        + request.js - axios的二次封装，用来发起接口请求，目前做了`toekn、loading、失败异常`的统一处理，无需在每个请求里边单独处理
    + mixins - 管理公共混入代码的目录
        + global - 全局混入,为防止重名，混入代码统一用大写字母、下划线分割(会在main.js文件里边统一引入，无需单独引入)
        + local - 局部混入,为防止重名，混入代码统一用大写字母、下划线分割(需要单独在组件中引入)
    + router - 管理页面路由的目录
    + services - 管理请求api的目录
    + store - 管理vuex的目录
    + utils - 管理js工具的目录
    + views - 页面存放的目录
        + 文件模式 - 用于一个页面，只有一个vue文件
        + 文件夹模式 - 用于一个页面，有多个vue文件，且其他文件不适合放到公共组件中（文件夹下，index.vue作为页面入口）

### 开发流程

+ 编辑器开启eslint、prettier插件
  
  ![img_4.png](docs/images/img_4.png)![img_3.png](docs/images/img_3.png)
  
+ 分支管理 - master（生产）、uat（预发）、st（测试）、dev（开发）
+ 数据Mock - TODO
+ 提交代码会自动执行gitHooks代码检查（commit后面添加--no-verify可跳过此环节，不推荐）
  + 现在gitHooks主要做了两件事情：
    1. 自动执行prettier与eslint的代码格式化，并做代码 lint报错检查，有报错则提交不通过
    2. 做git commit规范检查，会校验commit message内容，需符合以下规范才能提交

### git commit规范

+ Commit message格式
    + `<type>: <subject>`注意冒号后面有空格。

+ `type` 用于说明 commit 的类别，只允许使用下面7个标识。
    + feat：新功能（feature）
    + fix：修补bug
    + docs：文档（documentation）
    + style： 格式（不影响代码运行的变动）
    + refactor：重构（即不是新增功能，也不是修改bug的代码变动）
    + test：增加测试
    + chore：构建过程或辅助工具的变动
    
### 主题色使用
+ public/index.html文件中包含了主题色声明与切换的逻辑，现与方圆合规主题切换逻辑一致，只需在此基础上维护不同主题就行。
    + 主题色值的使用：
        1. `color: var(--primary-color);` 用css3的变量引用方式
        2. `color: $--primary-color;` 使用scss变量的引用方式，variables.scss文件声明了与public/index.html中对应的scss变量，并做了全局引入，使用时无需重复引入。

### 一些封装工具的使用示例
+ Create.js - 动态创建组件的方法
![img.png](docs/images/img.png)

+ eventBus.js - 事件总线，用于复杂场景的事件通讯
![img_3.png](docs/images/img_5.png)![img_4.png](docs/images/img_6.png)

+ request.js - axios的二次封装，用来发起接口请求，目前做了`toekn、loading、失败异常`的统一处理，无需在每个请求里边单独处理
    + 与直接调用[axios(config)]( https://github.com/axios/axios#axios-api )方式大概一致，只是增加了一些统一逻辑的处理
![img_2.png](docs/images/img_2.png)![img_1.png](docs/images/img_1.png)![img.png](docs/images/img_7.png)

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).
