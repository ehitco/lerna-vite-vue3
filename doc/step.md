# the log of create steps about this project

## lerna init

```js
npm i lerna -g
```

## creat vue3 demo

```js
lerna create @ehitco/test2
// lerna create vue3-demo
yarn create vite-app vue3-demo
```

## 分别给相应的 package 增加依赖模块

```js
// 一下仅为命令使用说明
lerna add chalk                                           // 为所有 package 增加 chalk 模块
lerna add semver --scope @mo-demo/cli-shared-utils        // 为 @mo-demo/cli-shared-utils 增加 semver 模块
lerna add @mo-demo/cli-shared-utils --scope @mo-demo/cli  // 增加内部模块之间的依赖
```

## 安装依赖

```js
// 使用 hoist 提升项目依赖，将所有依赖装到根目录
lerna bootstrap --hoist
```

## 为了省去每次都输入 --hoist 参数的麻烦，可以在 lerna.json 配置 yarn 模式不行

```json
{
  "packages": ["packages/*"],
  "command": {
    "bootstrap": {
      "hoist": true
    }
  },
  "version": "0.0.1-alpha.0"
}
```

## 配置好后，对于之前依赖包已经被安装到各个 package 下的情况，我们只需要清理一下安装的依赖即可

```js
lerna clean
```

## 启动 vue3 demo

> erna 不负责构建，测试等任务，它提出了一种集中管理 package 的目录模式，提供了一套自动化管理程序，让开发者不必再深耕到具体的组件里维护内容，在项目根目录就可以全局掌控，基于 npm scripts，使用者可以很好地完成组件构建，代码格式化等操作。接下来我们就来看看，如果基于 Lerna，并结合其它工具来搭建 Monorepo 项目的最佳实践。

### 一、优雅的提交

### 三、编译、压缩、调试

**采用 Monorepo 结构的项目，各个 package 的结构最好保持统一。**

根据目前的项目状况，设计如下：

1. 各 package 入口统一为 index.js
2. 各 package 源码入口统一为 src/index.js
3. 各 package 编译入口统一为 dist/index.js
4. 各 package 统一使用 ES6 语法、使用 Babel 编译、压缩并输出到 dist
5. 各 package 发布时只发布 dist 目录，不发布 src 目录
6. 各 package 注入 LOCAL_DEBUG 环境变量， 在 index.js 中区分是调试还是发布环境，调试环境 ruquire(./src/index.js) 保证所有源码可调试。发布环境 ruquire(./dist/index.js) 保证所有源码不被发布。

> 因为 dist 是 Babel 编译后的目录，我们在搜索时不希望搜索它的内容，所以在工程的设置中把 dist 目录排除在搜索的范围之外。

接下来，我们按上面的规范，搭建 package 的结构。
