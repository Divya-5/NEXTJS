# Learn about features of next js latest 3 major versions

## Next.js 14.1

Next.js 14.1 includes developer experience improvements including:

- Improved Self-Hosting: New documentation and custom cache handler
- Turbopack Improvements: 5,600 tests passing for next dev --turbo
- DX Improvements: Improved error messages, pushState and replaceState support
- next/image Improvements: <picture>, art direction, and dark mode support
- Parallel & Intercepted Routes: 20 bug fixes based on your feedback
- Handling cache especially in App Router.

I) For improved clarity on how to self-host Next.js with a Node.js server, Docker container, or static export.

1. Runtime environment variables
2. Custom cache configuration for ISR
3. Custom image optimization
4. Middleware

II) Turbopack Improvements

Turbopack is replacememt for webpack, it's a rust based compiler. It still being opt-in and there has been improvements in reliability performance and memory usage so if you are using it they have improved it in this new version but it is still remains as an opt-in option
so it's not yet replacing the webpack

III) DX Improvements (Developer Experience Improvements)
They have changed or improved error messages or have stack tracing in error messages in nextjs.
For ex:
Before if there was a failure inside your webpack or compiler

Before Image:
![Alt text](images/error-before.avif "first")

After Image:
![Alt text](images/error-after.avif "first")

API using the new APIs in the app router is that now you can use the push state and the replace state from the history object on the window so you can just directly call these two methods to manipulate the history stack inside of your browser without reloading the page. For ex:
If you want to use these two to sync with the usePathname and useSearchParams. For ex: To implement this sorting functionality which is going to be a query String or a search parameter inside a URL here we're calling this update sort functio passing it and string of the direction or sort order inside of the update sorting we are creating a URLSearchParams from the corent searchParams object that we get out of this hook we then set the sort to whatever sorting order that was passed to it and then we push this new state which is going to add this query string at the end of our current url because we are not changing the path here so we can use this inside of our app router without reloading the page 

`'use client';`\
 
`import { useSearchParams } from 'next/navigation';`\
 
`export default function SortProducts() {`\
  `const searchParams = useSearchParams();`\
 
  `function updateSorting(sortOrder: string) {`\
   ` const params = new URLSearchParams(searchParams.toString());`\
    `params.set('sort', sortOrder);`\
    `window.history.pushState(null, '', `?${params.toString()}`);`\
  `}`\
 
  `return (`\
    `<>`\
     ` <button onClick={() => updateSorting('asc')}>Sort Ascending</button>`\
     ` <button onClick={() => updateSorting('desc')}>Sort Descending</button>`\
    `</>`\
  `);`\
`}`\

Whether pushing a new state this allows the user to navigate back if they wanted to or replacing the current state which is not going to allow the user to navigate back to the previous state that they had becauses it replaces that state. 

Now another thing that I really like about logging functionality when you're running your local developement server is now some data cache logging so you can pass in a new config to your next-config.js so say logging fetches full URL this is going to log your full URL when you're running the development server but also its going to show you if you're hitting a cache or if you're skipping a cache and missing the cache this might be helpful if you're running different strategies and you want to test and try whether or not you're using the cache or you're hitting the cache this is going to actually log this information for you but this option only add this cache functionality or login if you're using the fetch and the cache for your fetch functions 

Terminal 
GET / 200 in 48ms
 ✓ Compiled /fetch-cache in 117ms
 GET /fetch-cache 200 in 165ms
  │ GET https://api.vercel.app/products/1 200 in 14ms (cache: HIT)
 ✓ Compiled /fetch-no-store in 150ms
 GET /fetch-no-store 200 in 548ms
  │ GET https://api.vercel.app/products/1 200 in 345ms (cache: SKIP)
  │  │  Cache missed reason: (cache: no-store)


  This can be enabled through next.config.js:

  `module.exports = {`\
  `logging: {`\
   ` fetches: {`\
    `  fullUrl: true,`\
   ` },`\
 ` },`\
`};`\


IV) next/image support for <picture> and Art Direction

The Next.js Image component now supports more advanced use cases through getImageProps() (stable) which don't require using <Image> directly. This includes:
* Working with background-image or image-set
* Working with canvas context.drawImage() or new Image()
* Working with <picture> media queries to implement Art Direction or Light/Dark Mode images

You can use there's actually two things here you can use the picture element now and you don't have to use necessarily the next/image component you can instead use this getImageProps function this is what allows you to use the picture element so as you can see here you're using this get getImageProps we passing in different Props nd right here if you're implementing a dark and a light version of our image depending on whether or not the user prefers a dark or light scheme 

`import { getImageProps } from 'next/image';`\
 
`export default function Page() {`\
 ` const common = { alt: 'Hero', width: 800, height: 400 };`\
 ` const {`\
   ` props: { srcSet: dark },`\
  `} = getImageProps({ ...common, src: '/dark.png' });`\
 ` const {`\
    `props: { srcSet: light, ...rest },`\
  `} = getImageProps({ ...common, src: '/light.png' });`\
 
  `return (`\
    `<picture>`\
     ` <source media="(prefers-color-scheme: dark)" srcSet={dark} />`\
     ` <source media="(prefers-color-scheme: light)" srcSet={light} />`\
      `<img {...rest} />`\
   ` </picture>`\
  `);`\
`}`\

V) Parallel & Intercepted Routes

Other thing which has improved significantly is a lot of boc fixes in parallel and intercepted routes. Parallel routes allows us ou to simultaneously they render one or more pages in the same layout so you can pull in two pages and show them inside a same layout. 
Intercepting routes is when you intercept a route ypu master URL and show the content of another page from another part of your application in the context of where the user is. 
For example - You're inside of a image gallery inside of a feed or a gallery user clicks on one single image and image would be shown inside of a modal without going to that specific page o you're bringing the content of another page and showing it in the context of the gallery so they're interesting patterns but they had a lot of box and now they have improved a lot of this box based on the community's feedback 

This next third party packages if you didn't know it now supports google analytics, google tag manager and google maps. Its very easy to use and optimises the script when and how it runs or how it loads so you don't have worry about loading this script inside of your application you just import this new component pass it to your ID your google tag manager ID or container ID and then it takes care of the rest As we can host fonts using the next font now they are bringing in a lot other third party packages that you would use typically in nextjs app into the next package 

`import { GoogleAnalytics } from '@next/third-parties/google'`
 
`export default function RootLayout({`
 ` children,`
`}: {`
  `children: React.ReactNode`
`}) {`
  `return (`
   ` <html lang="en">`
     ` <body>{children}</body>`
      `<GoogleAnalytics gaId="G-XYZ" />`
    `</html>`
  `)`
`}`

There are more improvements and additions to the documentation 

## Next.js 14

Next.js 14 is our most focused release with:
Server actions is stable. Server actions which is a way to mutate data in your server they have been experimental so far so you have to pass that flag in your config to use server actions for now but now they are fully stable 
* Turbopack: 5,000 tests passing for App & Pages Router
**** 53% faster local server startup
**** 94% faster code updates with Fast Refresh

* Server Actions (Stable): Progressively enhanced mutations
**** Integrated with caching & revalidating
**** Simple function calls, or works natively with forms

* Partial Prerendering (Preview): Fast initial static response + streaming dynamic content
* Next.js Learn (New): Free course teaching the App Router, authentication, databases, and more.

Terminal : npx create-next-app@latest

I) Next.js Compiler: Turbocharged

Turbopack which is a compiler they are using for nextjs it's a rust based compiler which they are making it faster for your local server setup for your hot module reloads and also fast refreshes for your code updates. 

Since Next.js 13, we've been working to improve local development performance in Next.js in both the Pages and App Router. It is basically stabilizing the new way that was introduced in nextjs 13 and that's mainly the app router builds on top of certain components. 

Previously, we were rewriting next dev and other parts of Next.js to support this effort. We have since changed our approach to be more incremental. This means our Rust-based compiler will reach stability soon, as we've refocused on supporting all Next.js features first.

5,000 integration tests for next dev are now passing with Turbopack, our underlying Rust engine. These tests include 7 years of bug fixes and reproductions.

While testing on vercel.com, a large Next.js application, we've seen:

Up to 53.3% faster local server startup
Up to 94.7% faster code updates with Fast Refresh
This benchmark is a practical result of performance improvements you should expect with a large application (and large module graph). With 90% of tests for next dev now passing, you should see faster and more reliable performance consistently when using next dev --turbo.

Once we hit 100% of tests passing, we'll move Turbopack to stable in an upcoming minor release. We'll also continue to support using webpack for custom configurations and ecosystem plugins.

II) Server Actions (Stable)

This feature is finally stable in this release as an effort to remove the developer experience the team behind nextjs wanted to provide an alternative to manually creating API routes  in next JS. This allows you to create a function directly in a component that's going to run on server. It can be used for catching, revalidating, redirecting and more in a single network round trip. Bit skeptical about server actions so is till need to test them out properly 

What if you didn't need to manually create an API Route? Instead, you could define a function that runs securely on the server, called directly from your React components.

The App Router is built on the React canary channel, which is stable for frameworks to adopt new features. As of v14, Next.js has upgraded to the latest React canary, which includes stable Server Actions.

`export default function Page() {`\
 ` async function create(formData: FormData) {`\
   ` 'use server';`\
   ` const id = await createItem(formData);`\
  `}`\

 ` return (`\
   ` <form action={create}>`\
     ` <input type="text" name="name" />`\
     ` <button type="submit">Submit</button>`\
    `</form>`\
 ` );`\
`}`\

Mutating data, re-rendering the page, or redirecting can happen in one network roundtrip, ensuring the correct data is displayed on the client, even if the upstream provider is slow. Further, you can compose and reuse different actions, including many different actions in the same route.

### Caching, Revalidating, Redirecting, and more
Server Actions are deeply integrated into the entire App Router model. You can:

Revalidate cached data with revalidatePath() or revalidateTag()
Redirect to different routes through redirect()
Set and read cookies through cookies()
Handle optimistic UI updates with useOptimistic()
Catch and display errors from the server with useFormState()
Display loading states on the client with useFormStatus()

####  III)Partial Prerendering (Preview)

They are introducing a new feature which is called partial pre-rendering this is still in preview so it is not even available in NextJS 14 yet but it will be something that we can benefit from later on and basically what it is it is going to allow you to statically render bits and pieces of your page at build time rather than exciting about a dynamic verus static page at page level for example if you have a suspense boundary that's holding up or suspending for a dynamic content to be fetched you typically have a fallback  and that fallback is a static that fallback or static fallback can be rendered at build time and instant shell can be shown to the user so at then end of the day your pages would render faster or would assume to be look seem to be rendered faster because the users are going to get that instant HTML or instant fallback or instant static parts of your page first before that Dynamic content is actually fetched and rendered on your page so it's a faster perceived page load and page load but also allows you to offload lot of those rendering to be done at build time so that you're just sending a static HTML so that's something that is coming in future.

Partial Prerendering builds on a decade of research and development into server-side rendering (SSR), static-site generation (SSG), and incremental static revalidation (ISR). Its core is compiler optimization build on top of react suspense where this suspense fallback will be pre-rendered. This is going to make the initial static response much faster which leads to all sorts of benefits from ux to SEO improvements.

Partial Prerendering requires no new APIs to learn.

#### Built on React Suspense
Partial Prerendering is defined by your Suspense boundaries. Here's how it works.  

app/page.tsx 

`export default function Page() {`\
 ` return (`\
   ` <main>`\
      `<header>`\
        `<h1>My Store</h1>`\
        `<Suspense fallback={<CartSkeleton />}>`\
         ` <ShoppingCart />`\
       ` </Suspense>`\
     ` </header>`\
     ` <Banner />`\
    `  <Suspense fallback={<ProductListSkeleton />}>`\
       ` <Recommendations />`\
     ` </Suspense>`\
      `<NewProducts />`\
    `</main>`\
  `);`\
`}`\

With Partial Prerendering enabled, this page generates a static shell based on your <Suspense /> boundaries. The fallback from React Suspense is prerendered.

Suspense fallbacks in the shell are then replaced with dynamic components, like reading cookies to determine the cart, or showing a banner based on the user.

When a request is made, the static HTML shell is immediately served:

`<main>`\
 ` <header>`\
   ` <h1>My Store</h1>`\
    `<div class="cart-skeleton">`\
      <!-- Hole -->\
    `</div>`\
  `</header>`\
 ` <div class="banner" />`\
  `<div class="product-list-skeleton">`\
    <!-- Hole -->\
  `</div>`\
 ` <section class="new-products" />`\
`</main>`\


Since <ShoppingCart /> reads from cookies to look at the user session, this component is then streamed in as part of the same HTTP request as the static shell. There are no extra network roundtrips needed.

app/cart.tsx
`import { cookies } from 'next/headers'`\
 
`export default function ShoppingCart() {`\
  `const cookieStore = cookies()`\
  `const session = cookieStore.get('session')`\
  `return ...`\
`}`\

To have the most granular static shell, this may require adding additional Suspense boundaries. However, if you're already using loading.js today, this is an implicit Suspense boundary, so no changes would be required to generate the static shell.

### Metadata Improvements
Before your page content can be streamed from the server, there's important metadata about the viewport, color scheme, and theme that need to be sent to the browser first.

Ensuring these meta tags are sent with the initial page content helps a smooth user experience, preventing the page from flickering by changing the theme color, or shifting layout due to viewport changes.

In Next.js 14, we've decoupled blocking and non-blocking metadata. Only a small subset of metadata options are blocking, and we want to ensure non-blocking metadata will not prevent a partially prerendered page from serving the static shell.

The following metadata options are now deprecated and will be removed from metadata in a future major version:

viewport: Sets the initial zoom and other properties of the viewport
colorScheme: Sets the support modes (light/dark) for the viewport
themeColor: Sets the color the chrome around the viewport should render with
Starting with Next.js 14, there are new options viewport and generateViewport to replace these options. All other metadata options remain the same.

You can start adopting these new APIs today. The existing metadata options will continue to work.


## Next.js 13.5

After the release from  NextJS 13.4 the previous version for me personally it felt a lot slower in development mode 
The job of build tool is to process and transform your source-code  into something that's optimiszed for browsers intoduction this is a complex process that includes making sure your code is compatible with older browser versions removing unnecessary characters splitting etc. 
NexusJs uses webpack as its build tool by default hot module replacement or HMR in short is the process of updating a part of the application without a full reload you probably see this often during development if you change just one part of your page and save your file it should automatically update the part that you were working on without reloading the whole page webpack has been around for a very long time and became a staple in web development however as web application and projects are becoming increasingly complex with more dependencies and plugins the performance of webpack seems to be decreasing resulting in longer build times and slower hot module replacements ever since the app router feature came out in version 13.4 next.js felt a lot slower when it comes to development mode and HMR also known as fast refresh especially since I was working with spelled kit lately which uses vite instead of that pack sure production mode was unaffected and continued to be blazingly fast but this simply isn't enough for a framework that's known for its improved developer experience.

Next.js 13.5 is all about performance optimization.

Next.js 13.5 improves local dev performance and reliability with:
22% faster local server startup: Iterate faster with the App & Pages Router

Since Next.js 13.4, our focus has been on improving performance and reliability for App Router applications. Comparing 13.4 to 13.5, we've seen the following improvements on a new application:

- 22% faster local server startup: Iterate faster with the App & Pages Router
- 29% faster HMR (Fast Refresh): For faster iterations when saving changes
- 40% less memory usage: Measured when running next start
- Optimized Package Imports: Faster updates when using popular icon and component libraries
- next/image Improvements: <picture>, art direction, and dark mode support

1. Optimized Package Imports: Faster updates when using popular icon and component libraries
   We've made an exciting breakthrough to optimize package imports, improving both local dev performance and production cold starts, when using large icon or component libraries or other dependencies that re-export hundreds or thousands of modules.

Previously, we added support for modularizeImports, enabling you to configure how imports should resolve when using these libraries. In
13.5, we have superseeded this option with optimizePackageImports, which doesn't require you to specify the mapping of imports, but instead will automatically optimize imports for you.

Libraries like @mui/icons-material, @mui/material, date-fns, lodash, lodash-es, ramda, react-bootstrap, @headlessui/react ,@heroicons/react , and lucide-react are now automatically optimized, only loading the modules you are actually using, while still giving you the convenience of writing import statements with many named exports.

2. next/image Improvements
   Based on community feedback, we've added a new experimental function unstable_getImgProps() to support advanced use cases without using the <Image> component directly, including:

- Working with background-image or image-set
- Working with canvas context.drawImage() or new Image()
- Working with <picture> media queries to implement Art Direction or Light/Dark Mode images

`import { unstable_getImgProps as getImgProps } from 'next/image';`\

`export default function Page() {`\
`  const common = { alt: 'Hero', width: 800, height: 400 };`\
 `const {`\
 ` props: { srcSet: dark },` \
 `} = getImgProps({ ...common, src: '/dark.png' });` \
 `const {`\
 `  props: { srcSet: light, ...rest },`\
 `} = getImgProps({ ...common, src: '/light.png' });`\

`return (`\
 `<picture>`\
 ` <source media="(prefers-color-scheme: dark)" srcSet={dark} />`\
 ` <source media="(prefers-color-scheme: light)" srcSet={light} />`\
 ` <img {...rest} />`\
 ` </picture>`\
 `);`\
`}`\
Additionally, the placeholder prop now supports providing arbitrary data:image/ for placeholder images that shouldn't be blurred

We were able to achieve this performance increase through optimizations like:

- Doing less work by caching or minimizing slow operations
- Optimizing expensive file system operations
- Better incremental tree traversal during compilation
- Moving unnecessary blocking synchronous calls to be lazy
- Automatically configuring large icon libraries

Next.js user Lattice reported between 87-92% faster compilation in their testing.
