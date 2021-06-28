## next.js  快学快用



### task1

过一遍官方文档

- next.js的Code splitting and prefetching。

  Next.js automatically optimizes your application for the best performance by code splitting, client-side navigation, and prefetching (in production).

  总之：就是不是全部页面都进行加载和渲染，而是一个一个页面来

-  add the `lang` attribute, you can do so by creating a `pages/_document.js` file. Learn more in the [custom `Document` documentation](https://www.nextjs.cn/docs/advanced-features/custom-document).

- 服务端渲染和静态生成，如果需要每次请求都进行更新的时候就用服务端渲染，类似博客这种展示类的用静态生成就ok了

- 静态生成依赖数据库等数据，使用export async function getStaticProps() {}去拉数据

- ts使用，根目录一个tsconfig.json 然后再yarn add一下，然后再yarn dev

一些类型：import { GetStaticProps, GetStaticPaths, GetServerSideProps } from 'next'

- 组件级css，Next.js 通过 `[name].module.css` 文件命名约定来支持 [CSS 模块](https://github.com/css-modules/css-modules) 。



### task2

过完了官方文档，应该去社区里面看一下具体的实践一起结合起来思考