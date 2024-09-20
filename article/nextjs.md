---
title: Nextjs
date: 2024-02-12 20:11:33
categories:
  - web
---

## 安装

```bash
npx create-next-app@latest
```

在提示之后，`create-next-app` 将创建一个带有项目名称的文件夹，并安装所需的依赖项。
运行`npm run dev` 启动本地服务`http://localhost:3000`

## 目录

对于新应用，我们建议使用 `App Router`。这个路由器允许你使用 React 的最新特性，是基于社区反馈的 `Pages Router` 的升级版。

### App Router

创建一个 `app/`文件夹，然后添加一个 `layout.tsx` 和 `page.tsx` 文件。这些将在用户访问应用程序的根目录(/)时呈现。

在`app/layout`中创建一个根布局。TSX 与所需的 html 和 body 标签:

```tsx
export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

创建页面 `app/page.tsx`

```tsx
export default function Page() {
  return <h1>Hello, Next.js!</h1>;
}
```

### Pages Router（可选）

如果你更喜欢使用 Pages Router 而不是 App Router，你可以在项目的根目录下创建一个 `pages/`目录。
添加 `index.tsx` 在 `pages` 目录中，即可通过/访问页面

```tsx
export default function Page() {
  return <h1>Hello, Next.js!</h1>;
}
```

添加`_app.tsx` 文件来定义全局布局

```tsx
import type { AppProps } from "next/app";

export default function App({ Component, pageProps }: AppProps) {
  return <Component {...pageProps} />;
}
```

`_document.tsx`来控制来自服务器的初始响应

```tsx
import { Html, Head, Main, NextScript } from "next/document";

export default function Document() {
  return (
    <Html>
      <Head />
      <body>
        <Main />
        <NextScript />
      </body>
    </Html>
  );
}
```

### 路由组

路由组可以通过在括号中包装一个文件夹来创建:`(xxxx)`

```text
app/
  page.js
  (admin)/
    login/
      page.js
  (about)/
    me/
      page.js
    other/
      page.js
```

路由组在以下情况下很有用:

- 将路线组织成组，例如按站点部分、目的或团队。
- 在同一路由段级别启用嵌套布局:
  - 在同一段中创建多个嵌套布局，包括多个根布局
  - 在公共段的路由子集中添加一个布

### public 文件夹（可选）

创建一个公用文件夹来存储静态资产，如图像、字体等。然后，您的代码可以从基础 URL(/)开始引用公共目录中的文件。

### 私有文件夹

可以通过在文件夹前加上下划线:`_xxx` 来创建私有文件夹

```text
app/
  page.js
  component/
  about/
    page.js
    _me/
      page.js
  _api/
  _lib/
```

因为默认情况下 app 目录下的文件可以安全地并置，所以不需要私有文件夹来进行并置。然而，它们可以用于:

- 将 UI 逻辑与路由逻辑分离。
- 避免潜在的命名冲突与未来的 Next.js 文件约定。
- 在代码编辑器中对文件进行排序和分组。
- 在项目和 Next.js 生态系统中持续组织内部文件。

您可以创建以下划线开头的 URL 段，方法是在文件夹名称前加上`%5F`(下划线的 URL 编码形式):`%5Fxxxx`。

## 路由

每个文件夹代表一个映射到 URL 段的路由段。要创建一个嵌套路由，您可以将文件夹相互嵌套。
然而，即使路由结构是通过文件夹定义的，路由也不能被公开访问，直到一个 page.js 或 route.js 文件被添加到路由段中。
而且，即使路由是可公开访问的，也只有 page.js 或 route.js 返回的内容才会被发送到客户端。
这意味着项目文件可以安全地放在 app 目录的路由段中，而不会意外地导致可路由。

```text
app/
  page.js
  test/
  about/
    page.js
    me/
      page.js
  api/
  lib/
```

一个特殊的 `page.js` 文件用于使路由段可公开访问(`.js`， `.jsx`， `.ts` 或`.tsx` 文件扩展名可用于特殊文件)。`/test` URL 路径是不可公开访问的，因为它没有相应的 `page.js` 文件。此文件夹可用于存储组件、样式表、图像或其他并置文件。

这与 `pages` 目录不同，在 `pages` 目录中，页面中的任何文件都被视为路由。
虽然你可以在应用程序中搭配你的项目文件，但你不必这样做。如果你愿意，你可以把它们放在 `app` 目录之外。

js 使用文件系统路由，这意味着应用程序中的路由是由你的文件结构决定的。

### `<Link>`

`<Link>`是一个内置组件，它扩展了 HTML <a>标签，在路由之间提供预取和客户端导航。这是 Next.js 中导航路由的主要和推荐的方式。

app/page.tsx

```tsx
import Link from "next/link";

export default function Page() {
  return <Link href="/dashboard">Dashboard</Link>;
}
```

Next.js 应用路由器的默认行为是滚动到新路由的顶部，或者保持滚动位置，以便向前和向后导航。如果你想禁用此行为，你可以将 scroll={false}传递给`<Link>`组件，或者将 scroll: false 传递给 router.push()或 router.replace()。

```tsx
// next/link
<Link href="/dashboard" scroll={false}>
  Dashboard
</Link>
```

### useRouter() hook

> 建议:使用`<Link>`组件在路由之间导航，除非你特别需要使用 useRouter。

useRouter 钩子允许你以编程的方式改变`客户端组件`的路由。

app/page.js

```tsx
"use client";

import { useRouter } from "next/navigation";

export default function Page() {
  const router = useRouter();

  return (
    <button type="button" onClick={() => router.push("/dashboard")}>
      Dashboard
    </button>
  );
}
```

### redirect 函数

对于服务器组件，请使用 redirect 函数。

`app/team/[id]/page.tsx`

```tsx
import { redirect } from "next/navigation";

async function fetchTeam(id: string) {
  const res = await fetch("https://...");
  if (!res.ok) return undefined;
  return res.json();
}

export default async function Profile({ params }: { params: { id: string } }) {
  const team = await fetchTeam(params.id);
  if (!team) {
    redirect("/login");
  }

  // ...
}
```

- redirect 默认返回 307(临时重定向)状态码。当在 Server Action 中使用时，它返回一个 303(参见 Other)，它通常用于作为 POST 请求的结果重定向到成功页面。

- Redirect 会在内部抛出错误，因此应该在 try/catch 块之外调用它。

- redirect 可以在呈现过程中在客户端组件中调用，但不能在事件处理程序中调用。你可以使用 useRouter 钩子代替。

- redirect 也接受绝对 url，可用于重定向到外部链接。

- 如果您想在呈现过程之前重定向，请使用 next.config.js 或 Middleware。

### 原生 History API

允许你使用原生的 window.history.pushState 和 window.history.replaceState 方法来更新浏览器的历史堆栈，而无需重新加载页面。
pushState 和 replaceState 调用集成到 Next.js 路由器中，允许你与 usePathname 和 useSearchParams 同步。

```tsx
"use client";

import { useSearchParams } from "next/navigation";

export default function SortProducts() {
  const searchParams = useSearchParams();

  function updateSorting(sortOrder: string) {
    const params = new URLSearchParams(searchParams.toString());
    params.set("sort", sortOrder);
    window.history.pushState(null, "", `?${params.toString()}`);
  }

  return (
    <>
      <button onClick={() => updateSorting("asc")}>Sort Ascending</button>
      <button onClick={() => updateSorting("desc")}>Sort Descending</button>
    </>
  );
}
```

App Router 使用混合的方式进行路由和导航。在服务器上，应用程序代码自动按路由段进行代码分割。在客户端，Next.js 预取和缓存路由段。这意味着，当用户导航到一个新路由时，浏览器不会重新加载页面，只有改变了的路由段才会重新渲染——从而改善了导航体验和性能。

1. 代码分割
   代码拆分允许您将应用程序代码拆分为更小的代码包，以供浏览器下载和执行。这减少了每个请求传输的数据量和执行时间，从而提高了性能。
   服务器组件允许应用程序代码自动按路由段进行代码拆分。这意味着导航只会加载当前路由所需的代码
2. 预取
   预取是在用户访问路由之前在后台预加载路由的一种方式。
   在 Next.js 中有两种方式预取路由:

- `<Link>`组件:路由在用户视图中可见时自动预取。预取发生在页面第一次加载或通过滚动进入视图时。
- `router.prefetch()`: useRouter 钩子可以用编程的方式预取路由。

`<Link>`的默认预取行为(即当预取道具未指定或设置为 null 时)根据您对 loading.js 的使用而有所不同。只有共享布局，沿着渲染的组件“树”一直到第一个加载.js 文件，会被预取并缓存 30 秒。这减少了获取整个动态路径的成本，这意味着你可以显示一个即时的加载状态，为用户提供更好的视觉反馈。

您可以通过将预取道具设置为 false 来禁用预取。或者，您可以通过将预取道具设置为 true 来预取超出加载边界的整页数据。

3. 缓存
   js 有一个内存中的客户端缓存，叫做路由器缓存。当用户在应用中导航时，预取的路由段和访问的路由的 React Server 组件负载存储在缓存中。
   这意味着在导航时，缓存被尽可能地重用，而不是向服务器发出新的请求——通过减少请求和数据传输的数量来提高性能。

4. 部分呈现
   部分渲染意味着只有在导航中改变的路由段才会在客户端上重新渲染，而所有共享的路由段都会被保留。
5. 软导航
   浏览器在页面之间导航时执行“硬导航”。Next.js 应用路由器支持页面间的“软导航”，确保只有改变过的路由段才会被重新渲染(部分渲染)。这使得客户端 React 状态可以在导航期间保留。
6. 前后导航
   默认情况下，Next.js 将为向后和向前导航保持滚动位置，并重用路由器缓存中的路由段。

### 动态路由

> 如果您事先不知道确切的段名，并且希望从动态数据创建路由，则可以使用在请求时填充或在构建时预呈现的动态段。

动态段可以通过将文件夹的名称用方括号括起来创建:`[folderName]`。例如`[id]`或`[slug]`。
动态段作为参数 prop 传递给 `layout`、`page`、`route` 和 `generateMetadata` 函数。

`app/blog/[slug]/page.tsx`

```tsx
export default function Page({ params }: { params: { slug: string } }) {
  return <div>My Post: {params.slug}</div>;
}
```

`/blog/a`===> `{ slug: 'a' }`

- 生成静态参数
  `generateStaticParams` 函数可以与动态路由段结合使用，在构建时静态生成路由，而不是在请求时按需生成路由
  `app/blog/[slug]/page.tsx`

```tsx
export async function generateStaticParams() {
  const posts = await fetch("https://.../posts").then((res) => res.json());

  return posts.map((post) => ({
    slug: post.slug,
  }));
}
```

`generateStaticParams` 函数的主要优点是它可以智能地检索数据。如果使用获取请求在 `generateStaticParams` 函数中获取内容，则会自动记住这些请求。这意味着跨多个 `generateStaticParams`、`Layouts` 和 `Pages` 使用相同参数的 `fetch` 请求将只进行一次，这减少了构建时间。

- 平级路由
  并行路由允许您同时或有条件地呈现同一布局中的一个或多个页面。它们对于应用程序的高度动态部分非常有用，例如仪表板和社交网站的提要。

并行路由是使用命名槽创建的。槽是用`@folder`约定定义的。例如，下面的文件结构定义了两个槽:`@analytics`和`@team`:

```text
app/
  @analytics/
    page.js
  @team/
    page.js
```

槽作为prop传递给共享的父布局。在上面的例子中，`app/layout.js` 中的组件现在接受`@analytics`和`@team` ，并且可以将它们与 `children` 并行渲染:
`app/layout.tsx`

```tsx
export default function Layout({ children, team, analytics }: { children: React.ReactNode; analytics: React.ReactNode; team: React.ReactNode }) {
  return (
    <>
      {children}
      {team}
      {analytics}
    </>
  );
}
```

但是，`slot`不是路由段，不会影响 URL 的结构。例如，对于`/@analytics/views`, URL 将是`/views`，因为`@analytics`是一个槽。
`children`属性是一个隐式槽，不需要映射到文件夹。这意味着`app/page.js`等同于`app/@children/page.js`。

### 活动状态和导航

默认情况下，Next.js 会跟踪每个插槽的活动状态(或子页面)。然而，槽内呈现的内容将取决于导航的类型:
软导航:在客户端导航期间，Next.js 将执行部分呈现，更改槽内的子页面，同时保持另一个槽的活动子页面，即使它们与当前 URL 不匹配。
硬导航:在整个页面加载(浏览器刷新)后，Next.js 无法确定与当前 URL 不匹配的插槽的活动状态。相反，它将呈现一个 default.js

不匹配路由的 404 有助于确保您不会意外地在页面上呈现非预期的并行路由。

您可以定义一个 default.js 文件，以便在初始加载或整个页面加载期间为不匹配的槽提供备用。
考虑下面的文件夹结构。@team 有一个/settings 页面，但是@analytics 没有。

```text
app/
  layout.js
  page.js
  default.js
  @analytics/
    default.js
    page.js
  @team/
    setting/
      page.js
```

当导航到/settings 时，@team 槽将呈现/settings 页面，同时保持@analytics 槽的当前活动页面。
在刷新时，Next.js 将为@analytics 呈现一个 default.js。如果 default.js 不存在，则会呈现 404。
此外，由于 children 是一个隐式槽，因此您还需要创建一个 default.js 文件，以便在 Next.js 无法恢复父页面的活动状态时为子页面呈现回退。
