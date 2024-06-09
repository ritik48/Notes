## What is NextJS ?

It is a React framework and usd to create web applications.

It was introduced because of some inconviniences in React
1.  In a React project, you have to maintain a separate Backend projects for your API routes.

2.  React does not provide out of the box routing (you have to use react-router-dom)

3.  React is **not SEO optimized**

4.  Waterfalling problem 
    when we access any page then we get all the components like, css, js html separately one after another, we can see this in network tab

### What NextJs ofers ?

1.  Server Side Rendering - Get's rid of SEO problems

2.  API routes - Single cdebase with frontend and backend

3.  File based routing (no need for react-router-dom)

4.  Bundle size optimization, static site generation

5.  Gives features like: Suspense, Server Components, Server Actions, streaming etc

6.  Maintained by the Vercel team


![alt text](image-3.png)


**Downsides :**

1.  Can't be distributed via a CDN, always needs a server running that does `server side rendering` and hence is expensive

2.  Very opinionated


---

### CSR vs SSR


| Feature                   | Client Side Rendering (CSR)                          | Server Side Rendering (SSR)                          |
|---------------------------|------------------------------------------------------|------------------------------------------------------|
| **Rendering**             | HTML is **rendered on the client** using JavaScript  | HTML is **rendered on the server**                   |
| **Initial Page Load Speed** | **Slower initial page loads**                            | **Faster initial page loads**                            |
|                           | - Bigger JavaScript bundle needs to be downloaded before app starts running | - Less JavaScript needs to be downloaded and executed |
|                           | - Data is fetched after components mount             | - Data is fetched before HTML is rendered             |
| **SEO Optimization**      | Not SEO optimized                                    | SEO friendly: Content is easier for search engines to index |


Two types of SSR:

1.  **Static :**

    HTML is generated at build time (often called Static Site Generation)

2.  **Dynamic :**

    HTML is generated each time server receives a new Request


![alt text](image-2.png)


---

### Hydration

When a React app is server side rendered, then the html which is generated is only the markup and does not have interactivity, i.e, like the event handlers which are attached to buttons are not there. This is solved by Hydration.

Hydration is the process of **transforming pre-rendered server-side** content **into an interactive user interface** on the client side.

How Hydration Works ?

1.  **Server-Side Rendering (SSR) :** 

    Next.js renders React components on the server in response to user requests, incorporating available data at that instance. This yields a fully rendered HTML page inclusive of server-derived state and data.

2.  **Send HTML to the Client :**

    Next.js transmits this fully rendered HTML page to the client's browser as the primary response. The HTML content encapsulates the static representation of the page.

3.  **Execution of Client-Side JavaScript :**

    Upon receiving the HTML, the client's browser commences executing the JavaScript bundles embedded within the page. These bundles reconstruct React components on the client side utilizing the same virtual DOM used on the server.


4.**Comparison and Reconciliation :**

    During rehydration, React on the client compares the server-rendered virtual DOM with its client-side counterpart, a process termed "reconciliation." React then synchronizes the client-side virtual DOM with its server-rendered counterpart.

    If there are differences between the sever rendered DOM and what the React generated on the client, then it will lead to Hydration error and bad user experience.

5.  **Even handling and Interactivity :**

    Following completion of the rehydration process, React components on the client side attain full interactivity


---

### Two was to add Routers in NextJs

1.  **App Router (Modern NextJS)**

    -   Introduced in NextJs 13.4 (2023)
    -   Implements **Reacts full stack architecure:** 
        -   Server Components
        -   Server Actions
        -   Streaming etc
    
    -   👍 **Easy fetching with fetch()** right int he components
    -   👍 Extremely easy to create **Layouts**, **Loaders**, etc
    -   👍 More advanced routing (Parallel routing, etc)
    -   👎 **Caching** is very aggressive and confusing

2.  **Pages Router (Legacy NextJS)**

    -   The first Nextjs router since v1 (2016)
    -   Still supported
    -   👍 Overall more **simple and easy** to learn
    -   👎 Simple things like **layouts** are confusing to implement
    -   👎 Data fetching using Nextjs specific apis such as: **getStaticProps and getServerSideProps**


---

### Creating a Next App

`npx create-next-app@latest folder-name`
