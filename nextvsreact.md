# How NextJS is different from React?

Simplifies the development process. On top of it optimises your web Apps. 

* Rendering
The primary distinction b/w React and NextJS lies in how they handle rendering. You may already know ReactJS renders user interface on Client Side while NextJS performs Server Side Rendering however NextJS provides flexibility in rendering options. You can choose to render the UI on the client siide or the server side according to your needs. 

## SEO 
Client Side Rendering or Browser Rendering happens on client's device or the browser when user requests a webpage the server sends a basic HTML document and JS code the browser then downloads and executes the JS code which leads to the rendering of components and finally display the website. Search Engine crawlers face difficulties indexing pages dynamically rendered on client side as a result the SEO performance of such pages may suffer as search engines may not fully comprehend their content and rank them appropriately. React seo score is very bad. On crawling load, API calling happens within 2-3 seconds data is fetched and re-render the components and data will display. Bot will think that page is empty and no data on that. SEO score will go down.

Server Side Rendering - It involves rendering the webpage on server before transmitting it to the client's device when a user requests a page the server processes the request and render the components on server side. The server side then sends back the fully rendered HTML to the client's browser enabling immediate display this distinction highlights an aspect of web development SEO. By using NextJS this issue is resolved by sending pre-rendered code directly to client. This enables easy crawling and indexing by search engines leading to the improved SEO. SEO is crucial for optimising website's visibility and ranking in Search Engine results by focussing on SEO several benefits. 
### Increased Organic Traffic
### Enhanced user experience
### Credibility and Trustworthiness
### Competitive advantage

Priortizing SEO can greatly impact the success of your website and its online presence. With NextJS Approach data is already on screen because data is fetched during build time and NextJS has created it HTML and CSS. If google bots come here so in first load it will see the data and it will be able to index it so this is the benefit of using NextJS. 

## Build
When you create a build of React App - JS files are created . In React development mode you createapp and then you deploy build on server. User comes and open your website. Code is written in React. JS bundle will load on your browser and ReactJs bundle is very heavy so there will some waiting time for user which is not a correct thing. 
React App will load. UseEffect Hooks will run and all Api will fetch data and after data is there then state will be updated and components will re-render the data and then only the user can intereact with your website. From user prespective it is very time consuming which is not good for SEO and user experience. Loading speed of website should be fast and React is client side rendering so solve it is very difficult. 
NextJS internally uses React. When you create a build, it fetch all the data which has to be loaded later from API calls during build time it fetches & directly creates HTML and CSS files. Deploy HTML and CSS on server . Whe user comes and visit your website. It opens HTML & CSS will load which is really fast as JS is heavy and as data is pre-fetched so user see data instantly on the screen. Hence reducing load time on website. 

React internally uses webpack and webpack takes time to bundle up the code. NextJS uses RUST internally that is very fast in comparison to webpack. Compile time & build is very fast. NextJS is build by vercel.com so vercel.com is a Hoisting provider site and CI/CD deployment is very easy. So CI/CD deployment & hositing is free 

## Routing
How do we create different Page Routes in React?
An additional Package called React Router DOM & create routes in one of the files. For creating multiple pages in ReactJS you need to use React Router DOM Library.

In NextJS it is build in. GetStaticProps, GetStaticPaths, Dynamic Routing, SSR, CSR. NextJS uses file based Routing System. The routing is handled by the file system. Each folder in app directly become a route & folder name becomes the route path. Eg: No need of external packages and complex configurations. You can create files for the routes you want and immediately open them within your application. 

## Ability to ceate full stack applications?
NextJS 9 features are behind NextJS introduced a new feature API routes. Enabling the creation of serverless functions to handle API requests. Serverless API in NextJS are a way of creating API endpoints without the need for a traditional server. It allows us to build and deploy APIs. 
* Without managing Server Infrastructure 
* Worrying about scaling their server as traffic increases. 
With this feature we can create API endpoint by simply creating route.js in a specific folder within the app directory. This file in any route segment of app directly corresponds to that route API endpoint once.

## Automatic Code Spiltting 
Code Spilitting is a technique that breaks down large bundles of JS code into smaller, more manageable chunks that can be loaded as needed. This reduces the initial load time of a website & optimises user experience while browsing.
