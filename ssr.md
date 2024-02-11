# Server-side Rendering (SSR) or "Dynamic Rendering"
With SSR, you can render the JavaScript code on the server and send indexable HTML to the user. The HTML is thus generated during runtime so that it can reach search engines and users at the same time.

Dynamic Server rendered data is fetched fresh on each request with SSR. Each request to the server triggers a new rendering cycle & data fetch ensuring that the component is always up to date here.

SSR pages are generated upon each request. The logic behind SSR is executed only on the server-side. It never runs in the browser.

If a page uses Server-side Rendering, the page HTML is generated on each request.
To use Server-side Rendering for a page, you need to export an async function called getServerSideProps. This function will be called by the server on every request.

For example, suppose that your page needs to pre-render frequently updated data (fetched from an external API). You can write getServerSideProps which fetches this data and passes it to Page like below:

`export default function Page({ data }) {`
  // Render data...
`}`
 
// This gets called on every request
`export async function getServerSideProps() {`
  // Fetch data from external API
  `const res = await fetch(`https://.../data`)`
  `const data = await res.json()`
 
  // Pass data to the page via props
  `return { props: { data } }`
`}`

As you can see, getServerSideProps is similar to getStaticProps, but the difference is that getServerSideProps is run on every request instead of on build time.

getServerSideProps is a Next.js function that can be used to fetch data and render the contents of a page at request time.

Example:
You can use getServerSideProps by exporting it from a Page Component. The example below shows how you can fetch data from a 3rd party API in getServerSideProps, and pass the data to the page as props:

`export async function getServerSideProps() {`
  // Fetch data from external API
  `const res = await fetch('https://api.github.com/repos/vercel/next.js')`
  `const repo = await res.json()`
  // Pass data to the page via props
  `return { props: { repo } }`
`}`
 
`export default function Page({ repo }) {`
  `return (`
    `<main>`
      `<p>{repo.stargazers_count}</p>`
    `</main>`
  `)`
`}`


## When should I use getServerSideProps?
You should use getServerSideProps if you need to render a page that relies on personalized user data, or information that can only be known at request time. For example, authorization headers or a geolocation.

## Behavior
- getServerSideProps runs on the server.
- getServerSideProps can only be exported from a page.
- getServerSideProps returns JSON.
- When a user visits a page, getServerSideProps will be used to fetch data at request time, and the data is used to render the initial HTML of the page.
- props passed to the page component can be viewed on the client as part of the initial HTML. This is to allow the page to be hydrated correctly. Make sure that you don't pass any sensitive information that shouldn't be available on the client in props.
- When a user visits the page through next/link or next/router, Next.js sends an API request to the server, which runs getServerSideProps.
- You do not have to call a Next.js API Route to fetch data when using getServerSideProps since the function runs on the server. Instead, you can call a CMS, database, or other third-party APIs directly from inside getServerSideProps.

The getServerSideProps function always returns an object with one of the following properties:
* props – contains all the data required to render a page.
* notFound – allows the page to return a 404 status and a 404 Page.
* redirect – allows redirecting the user to internal and external resources.

## Error Handling
If an error is thrown inside getServerSideProps, it will show the pages/500.js file.During development, this file will not be used and the development error overlay will be shown instead.

## Edge Cases

### Edge Runtime
getServerSideProps can be used with both Serverless and Edge Runtimes, and you can set props in both.

However, currently in the Edge Runtime, you do not have access to the response object. This means that you cannot — for example — add cookies in getServerSideProps. To have access to the response object, you should continue to use the Node.js runtime, which is the default runtime.

You can explicitly set the runtime on a per-page basis by modifying the config, for example:

export const config = {
  runtime: 'nodejs', // or "edge"
}
 
export const getServerSideProps = async () => {}

### Caching with Server-Side Rendering (SSR)
You can use caching headers (Cache-Control) inside getServerSideProps to cache dynamic responses. For example, using stale-while-revalidate.

// This value is considered fresh for ten seconds (s-maxage=10).
// If a request is repeated within the next 10 seconds, the previously
// cached value will still be fresh. If the request is repeated before 59 seconds,
// the cached value will be stale but still render (stale-while-revalidate=59).
//
// In the background, a revalidation request will be made to populate the cache
// with a fresh value. If you refresh the page, you will see the new value.
export async function getServerSideProps({ req, res }) {
  res.setHeader(
    'Cache-Control',
    'public, s-maxage=10, stale-while-revalidate=59'
  )
 
  return {
    props: {},
  }
}

However, before reaching for cache-control, we recommend seeing if getStaticProps with ISR is a better fit for your use case.

The major difference between SSR and SSG is that in the latter’s case, rather than during runtime, your HTML is generated during build time. Such websites are extremely fast since the HTML content is served even before you make a request. On the other hand, the website needs to be rebuilt and reloaded entirely every time a change is made. Consequently, SSG-based websites are far less interactive and native-like than those that rely on SSR. They are largely static sites with little to no dynamic content.


## When to use SSR?
SSR is recommended for apps in which you have to pre-render frequently updated data from external sources. This technique is especially recommended when the data cannot be statically generated before a user request takes place, and at the same time needs to be available to search engines.
Example
A search results page or an e-commerce website that includes user-generated content.

### Pros of SSR:
* the page always contains up-to-date content,
* if an error is thrown inside getServerSideProps, Next.js will show an error page automatically
* you have access to cookies, request headers, and URL query parameters
* you can implement logic related to the 404 page, and redirects based on user requests and data

### Cons of SSR:
* the page is noticeably slower than a statically generated one because some logic needs to be executed on every request (e.g. API call).
* a server-side rendered page can be cached on CDNs only by setting the cache-control header (it requires additional configuration).
* the Time to First Byte (TTFB – the time it takes a server to deliver the first byte of information to a page) metric will be higher than on a statically generated page