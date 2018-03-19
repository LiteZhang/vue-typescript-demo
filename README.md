# ts-vue-project

> A Vue.js project with TypeScript

## 如何在Vue项目中引入TypeScript
1. 在vue脚手架的基础上执行以下命令安装ts相关依赖以及vue官方推荐的装饰器，注意：webpack如果不是4以上版本ts-loader要安装3.5.0以下版本，否则会编译失败！

```bash
npm i ts-loader typescript --save-dev

npm i vue-class-component vue-property-decorator --save
```

2. 将 `src/main.js` 改成 `src/main.ts`, 修改 `bulid/webpack.base.conf.js`配置，在文件内entry、resolve和rules配置项中修改或增加以下代码：
```js
    entry: {
        app: './src/main.ts'
    }
    ...
    resolve: {
        extensions: ['.js', '.vue', '.json', '.ts'],
        alias: {
        '@': resolve('src')
        }
    }
   ...
   //ts-loader 会检索当前目录下的 tsconfig.json 文件，根据里面定义的规则来解析.ts文件（就跟.babelrc的作用一样）
    {
        test: /\.tsx?$/,
        loader: 'ts-loader',
        exclude: /node_modules/,
        options: {
            appendTsSuffixTo: [/\.vue$/],
        }
    }cak
```
3. 添加tsconfig.json文件，具体配置自行查看[https://www.tslang.cn/docs/handbook/compiler-options.html]
```json
    {
        "include": [
            "./src/**/*"
        ],
        "exclude": [
            "node_modules"
        ],
        "compilerOptions": {
        //"strictPropertyInitialization": false,
        //以严格模式解析
        "strict": true,
        //在.tsx文件里支持JSX
        "jsx": "preserve",
        //使用的JSX工厂函数
        "jsxFactory": "h",
        //允许从没有设置默认导出的模块中默认导入
        "allowSyntheticDefaultImports": true,
        //启用装饰器
        "experimentalDecorators": true,
        //允许编译javascript文件
        "allowJs": true,
        //采用的模块系统
        "module": "esnext",
        //编译输出目标 ES 版本
        "target": "es5",
        //如何处理模块
        "moduleResolution": "node",
        //在表达式和声明上有隐含的any类型时报错
        "noImplicitAny": true,
        "lib": [
            "dom",
            "es5",
            "es6",
            "es7",
            "es2015.promise"
        ],
        "sourceMap": true,
        "pretty": true
    }
  
```
4. 由于 `TypeScript` 默认并不支持 `*.vue` 后缀的文件，所以在 vue 项目中引入的时候需要创建一个 vue-shims.d.ts 文件，放在项目 `/src` 目录下，告诉 TypeScript *.vue 后缀的文件可以交给 vue 模块来处理。而在代码中导入 *.vue 文件的时候，需要写上 .vue 后缀。因为 TypeScript 默认只识别 *.ts 文件，不识别 *.vue 文件，代码如下

```js
    declare module "*.vue" {
        import Vue from "vue";
        export default Vue;
    }
```

5. 改造vue文件，[vue-class-component](https://github.com/vuejs/vue-class-component) 对 `Vue` 组件进行了一层封装，让 Vue 组件语法在结合了 TypeScript 语法之后更加扁平化:

```ts
    <script lang="ts">
        import Vue from 'vue'
        import Component from 'vue-class-component'
        @Component({
            props: {
                propMessage: String
            }
        })
        export default class HelloWorld extends Vue {
            // initial data
            msg: string = 'This is a typescript project now'
            // lifecycle hook
            mounted () {
                this.greet()
            }
            // computed
            get computedMsg () {
                return 'computed ' + this.msg
            }
            //method
            greet () {
                alert('greeting: ' + this.msg)
            }    
        }
    </script>
```
6. [vue-property-decorator](https://github.com/kaorun343/vue-property-decorator) 是在 vue-class-component 上增强了更多的结合 Vue 特性的装饰器，新增了这 7 个装饰器：
* `@Emit`
* `@Inject`
* `@Model`
* `@Prop`
* `@Provide`
* `@Watch`
* `@Component` (exported from vue-class-component)

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

```