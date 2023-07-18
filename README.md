开始了所谓 OneDayOneToy 的独立项目，for fun. 

希望自己每天能撸一个最简单的小组件，并能直接在这个 site 上被渲染出来。还要有代码编辑器能看到这个小组件的代码。

每天也可以写一段话，画一个图，写一道题，写一篇对于某个技术的理解。反正就是有趣的。

让生活可量化一点？别白过了（突然矫情）

## 2023/06/27
1、接触了 next.js 的皮毛。用项目结构的约束来定义路由。next.js 的好处主要是组件都是服务端渲染的组件，所以说今天只学到了个皮毛。

https://www.bilibili.com/video/BV1Mo4y1T78C/?spm_id_from=333.337.search-card.all.click&vd_source=9db8fb9fbd6a9422513f20fc6e7ee114

这里学的，只看了 1/7 。看的时候发现自己 css 基础知识也很欠缺，比如什么 justify-content 的属性就完全没听说过。给自己留了个课后作业，要补一补 css。
https://www.imooc.com/learn/33 这个可以一天敲一节。

https://github.com/onfuns/nestjs-blog/tree/master 这个项目感觉不错，学差不多了可以拆解一下。

明天需要花半小时再看一下 next.js 的项目，花半小时敲一下 css，再花两小时实现一个这样的功能：设定大目标与拆解出来的小目标，都可以分别统计进度，大目标汇总小目标的进度。

## 2023/06/30
最近跟着b站那个视频继续做一个site，跟着敲也能学到东西。最近的目标是跟着这个敲完。

## 2023/07/19
昨天百思不得其解，为什么在 playwright 里的 testcase 里，加一个引用本地其他模块的 import 就会报错？且 IDE 没有报错。

怀疑说 playwright 在执行时用了自己的 tsconfig，使用了不同的 baseUrl（因为引用 playwright 模块时用了个别名）

今天一下找到原因了，在引用本地模块时需要加上本地模块所属文件的扩展名。

研究了一下，这个是 ESM 的引用方式（ESM: import/export CJS: require/module.exports/exports），而 node 的官方文档里明写着“以相对路径或绝对路径引用模块时需要明确扩展名”。但是，实际项目中很多引用并没有扩展名啊！故查，什么情况下需要扩展名，什么情况下不需要。

结果是：在浏览器原生环境里 import 是需要明确指定扩展名的。经过打包工具转译的 case 中，则不需要指定扩展名（会由打包工具转译为 cjs 的形式）。

那使用npx playwright test 的命令时，是使用了 ESM 的模块引用方式吗？于是看 playwright 的源码。

粗浅地看了，项目太复杂了。大概是有一个 testFileLoader，扫描项目所需的依赖并加载，扫描项目里包含的测试回调函数，并调用该函数，以此完成测试。

那么使用npx，就是在 nodejs 环境中执行 node 代码咯，那当然是在浏览器原生环境执行代码。所以，引用时不加文件扩展名，执行时是不认识的。