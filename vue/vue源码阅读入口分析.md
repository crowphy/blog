# Vue源码阅读
======

由于从 github 上面克隆的 vue 项目比较庞大，一般一时无法准确找到入口文件。但是可以从构建工具下手，一步步查找。

#### 第一步：查看`package.json`文件，通过它可以知道 vue 里面使用了哪些脚本命令。
<pre><code>"scripts": {
    "dev": "rollup -w -c build/config.js --environment TARGET:web-full-dev",
    "dev:cjs": "rollup -w -c build/config.js --environment TARGET:web-runtime-cjs",
    "dev:esm": "rollup -w -c build/config.js --environment TARGET:web-runtime-esm",
    "dev:test": "karma start test/unit/karma.dev.config.js",
    "dev:ssr": "rollup -w -c build/config.js --environment TARGET:web-server-renderer",
    "dev:compiler": "rollup -w -c build/config.js --environment TARGET:web-compiler ",
    "dev:weex": "rollup -w -c build/config.js --environment TARGET:weex-framework",
    "dev:weex:factory": "rollup -w -c build/config.js --environment TARGET:weex-factory",
    "dev:weex:compiler": "rollup -w -c build/config.js --environment TARGET:weex-compiler ",
    "build": "node build/build.js",
    "build:ssr": "npm run build -- web-runtime-cjs,web-server-renderer",
    "build:weex": "npm run build -- weex",
    "test": "npm run lint && flow check && npm run test:types && npm run test:cover && npm run test:e2e -- --env phantomjs && npm run test:ssr && npm run test:weex",
    "test:unit": "karma start test/unit/karma.unit.config.js",
    "test:cover": "karma start test/unit/karma.cover.config.js",
    "test:e2e": "npm run build -- web-full-prod,web-server-basic-renderer && node test/e2e/runner.js",
    "test:weex": "npm run build:weex && jasmine JASMINE_CONFIG_PATH=test/weex/jasmine.json",
    "test:ssr": "npm run build:ssr && jasmine JASMINE_CONFIG_PATH=test/ssr/jasmine.json",
    "test:sauce": "npm run sauce -- 0 && npm run sauce -- 1 && npm run sauce -- 2",
    "test:types": "tsc -p ./types/test/tsconfig.json",
    "lint": "eslint src build test",
    "flow": "flow check",
    "sauce": "karma start test/unit/karma.sauce.config.js",
    "bench:ssr": "npm run build:ssr && node benchmarks/ssr/renderToString.js && node benchmarks/ssr/renderToStream.js",
    "release": "bash build/release.sh",
    "release:weex": "bash build/release-weex.sh",
    "release:note": "node build/gen-release-note.js",
    "setup": "node build/setup.js",
    "commit": "git-cz"
  }</code></pre>
  
  现在看看`dev`命令做了什么。通过 rollup（类似 webpack 的打包工具）执行`build/config.js`文件，并附带了`TARGET:web-full-dev`参数。接下来就去`build/config.js`文件里看看有什么内容。
  ------
####   第二步：查看`build/config.js`文件

 通过搜索`TARGET`可以发现一进来就执行了`genConfig(process.env.TARGET)`这个方法，在该方法里可以看到`const opts = builds[name]`，然后寻找`builds `，可以发现这个对象里就包含了上述`scripts`所有命令所需要的入口文件配置。
 比如：
 <pre><code>// Runtime+compiler development build (Browser)
  'web-full-dev': {
    entry: resolve('web/entry-runtime-with-compiler.js'),
    dest: resolve('dist/vue.js'),
    format: 'umd',
    env: 'development',
    alias: { he: './entity-decoder' },
    banner
  }</code></pre>
  就是打包开发环境所需要的配置。然后就找到`web/entry-runtime-with-compiler.js`，也就是 vue 的入口文件。接下来就从这个文件出发一步步发掘 vue 的运行逻辑。