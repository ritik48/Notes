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


### React Server Component (RSC)

- A full stack architecture for React apps

- Introduces server as an integral part of React component trees: **server components**

- We write **frontend code** next to **backend code**

- RSC is **not active by default** in new React apps (e.g. Vite apps):
    it needs to be **implemented** by a framework like **Nextjs** (app router)

The component we write in nextjs are server component by default.

**Client Component :**

- Regular components
- Created with `"use client"` directive at the top of the module
- They are typically rendered on the client-side (CSR) but, they can also be rendered to HTML ont he server (SSR), allowing users to immediately see the page's HTML conetnt rather than a blank screen

- Have access to client environment, such as the browsers, allowing them to use state, effects and vent listeners to handle interactivity and also access browser-exclusive APIs like geolocation or localStorage, allowing you to build UI for specific use cases.



**Server components :**

-   **Compoenents that are only rendered on the server**
- And unlike client components, their code stays on the server and is never donloaded to the client
-   Don't make it into the bundle
-   We can build the backend with React
-   **Default** in apps that use the RSC architecture (Nextjs)

Benefits of SErver components

- **Reduced Bundle Sizes**
    Server components do not send code to the client, allowing large dependencies to remain server-side

    This benefit users with slower internet connections be eliminating the need to download, parse and execute Javascript for these components

- **Enahanced security**
    It enhances security by keeping sensitive data and logic, including tokens and API keys, away from the client-side

- **Improved Data Fetching**

- **Caching**
    Rendering on the server enables caching of results, which can be reused in subsequent requests and across different users

- **Efficient Streaming**
    Server components allow the rendering process to be divided into manageable chunks, which are then streamed to the client as soon as they are ready

    This allows users to start seeing parts of the page earlier, eliminating the need to wait for the entire page to finish rendering on the server

- **Improved SEO**

![alt text](image-4.png)

![alt text](image-5.png)

#### Why RSC Payload ? Why not render Server Component's as HTML ?

- **When a SC is re-rendered:** React is able to merge ("reconcile") the **current** tree on the client with a **new tree** coming from the server

- As a result, UI **state can be preserved** when a SC re-renders, instead of completely re-generating the pages as HTML


--- 

#### Understanding App router

Inside app folder, if there is a `page.tsx` file, then this is what will be rendered on the root route.

Now, to make different routes, create a folder and inside that folder put page.tsx
i.e.,

- app
    - About
        - page.tsx
    - page.tsx

According to above structure , we have two routes here:
1.  /
2.  /About

Now, to avoid a folder name in the route, create a folder and name it inside the brackets.

- app
    - (auth)
        - signin
            - page.tsx
        - signup
            - page.tsx

Here, we can access signin and signup as, `/signin` and `/signup`, no need to include `auth` in the path.

#### Layouts

Every route can also have layouts. To create a layout use `layout.tsx` as a file name.

And, here other components can be passed as a children props.

#### Loading

To show loading state when a server component is taking time fetching data, we can show a loading state.
For this as well, we have a convention to create a loading state.

In Whatever route you feel it will take time to load a component, you can create a `loading.tsx` file with the loader component in it.

And that's it, it will automatically show the loading state when a component is taking time to be displayed.

### File Conventions

Next.js provides a set of special files to create UI with specific behavior in nested routes:

Files                   | Behavour                |  
|-----------------------|-------------------------|
layout	                | Shared UI for a segment and its children |
page	                | Unique UI of a route and make routes publicly accessible
loading	                | Loading UI for a segment and its children
not-found	            | Not found UI for a segment and its children
error	                | Error UI for a segment and its children
global-error	        | Global Error UI
route	                | Server-side API endpoint
template	            | Specialized re-rendered Layout UI
default	                | Fallback UI for Parallel Routes


### Root Layout (Required)

The root layout is defined at the top level of the app directory and applies to all routes. This layout is required and must contain html and body tags, allowing you to modify the initial HTML returned from the server.

### Navigation

1. #### `useRouter()` hook

    The useRouter hook allows you to programmatically change routes from Client Components.

    ```
    'use client'
    
    import { useRouter } from 'next/navigation'
    
    export default function Page() {
    const router = useRouter()
    
    return (
        <button type="button" onClick={() => router.push('/dashboard')}>
        Dashboard
        </button>
    )
    }
    ```

2. #### `redirect` function

    For Server Components, use the redirect function instead.

    ```
    import { redirect } from 'next/navigation'
    
    async function fetchTeam(id: string) {
        const res = await fetch('https://...')
        if (!res.ok) return undefined
        return res.json()
    }
    
    export default async function Profile(
        { params }: { params: { id: string } }) {
        const team = await fetchTeam(params.id)
            if (!team) {
                redirect('/login')
            }
        }
    
    // ...
    ```

### Dynamic Routes (including the params in the route)

Convention
A Dynamic Segment can be created by wrapping a folder's name in square brackets: `[folderName]`. 

For example, `[id]` or `[slug]`.

For example, a blog could include the following route `app/blog/[id]/page.js`

```
export default function Page({ params }) {
  return <div>My Post: {params.slug}</div>
}
```

Route	                        | Example URL	            | params
--------------------------------|---------------------------|------------------
app/blog/[slug]/page.js	        | /blog/a	                | { slug: 'a' }

---



## Excluding a folder from acting as Route

Any folder we create inside `app` will be considered as route path

Let's say we have a folder `home` inside `app` then if there is a `page.js` inside this `home` folder, then we can access it like `/home`.

To avoid `home` to be accessible, prefix the folder name with underscore `_home`


### Catch all handler route

let's say we want to catch routes like this `/course/anything/anything`

i.e., after `/course` doesn't matter what comes, I want to catch that

For this the route folder structure will be:

- app
    - course
        - [...courseID]
            - page.jsx

Now, we can extract that path after course, using the `params`, that the handler in the page.jsx receives.


## API routes in NextJs

Nextjs lets you wite backend routes, just like express does.
This is why next is considered as a `full stack` framework.

The benefits of using NextJs for a backend includes:

1.  Code in a single repo
2.  All standard things you get in a backend framework like express.
3.  Server components can directly talk to the backend.

### Let's create a route which return some data

So, to setup backend routes in NextJs, we have to follow these coventions:

1.   Create `api` folder inside `app`
2.   For each route create a folder 
    
    Let's say we want to create `/user` route 
    Then, we will have to create a folder `user` and iside this we will have a file `route.ts`.

    -   app
        -   api
            -   user
                -   route.ts
3.  We can create different route handlers inside this `route.ts`

    - GET Route
        ```
        export function GET(req: NextRequest) {
            const body = req.json()
            
            Response.json({name: "Ritik", email: "ritik@gmail.com"})
        }
        ```
        Like wise for POST, PUT routes

---

## Server Actions

Server Actions are asynchronous functions that are executed on the server. They can be used in Server and Client Components to handle form submissions and data mutations in Next.js applications.


Server Components can use the inline function level or module level "use server" directive. To inline a Server Action, add "use server" to the top of the function body:

e.g.

```
function getPost() {
    "use server";

    // get post

    return post;
}

```

Now the way we have created above server action function can be used only by server component. To use it in client components as well we need to use the module-level `"use server"` directive.

```
"use server"

export async function create() {
  // ...
}
```

---

## NextAuth

It is a library which helpls to setup auth in nextjs.

We can setup all kind of authentication like email based, google, facebook, etc

### How to setup ?

First we need to create `/api/auth` route and give NextAuth access to catch-all route basically any request ending with `/api/auth` will be handled by NextAuth

So, we will create api route like this:

-   app
    -   api
        - auth
            - [...nextAuth]
                - page.tsx

Inside page.tsx

We will create `handler` and `provider`

```

import NextAuth from "next-auth"

const handler = NextAuth({
    // here we have to provide the providers with which we want to authneticate
})

export const GET = handler;
export const POST = handler;

```

We will use `CredentialsProvider`. It allows us to setup authentication using arbitrary credentials, meaning we can structure our authnetication however we like.

```
import NextAuth from "next-auth"
import CredentialsProvider from "next-auth/providers/credentials"

const handler = NextAuth({
    providers: [
        CredentialsProvider(
            {
                // The name to display on the signin form. Like "Sign in with..."
                name: "EmailAuth"

                // The credentials is used to genrate a suitable form on the signin page.
                // You can specify whatever fields you are expecting to be submitted.
                // 
                credentials: {
                    username: {
                        label: "Username", type: "text", placeholder: "username"
                    },
                    password: {
                        label: "Password", type: "password", placeholder: "password"
                    }
                }
                async authorize(credentials, req) {

                    // here will be the logic of handling the user submitted data
                    // and return the data that will contain user info

                    // if we return false/null from here, it will mean that there was error while authentication and this error is automatically displayed by next-auth on the signin page.

                    retutn {
                        id: 1
                    }
                }
            }
        )
    ],
    secret: process.env.NEXTAUTH_SECRET
})

export const GET = handler;
export const POST = handler;

```

The `id` in the object that authorize function returned will not be present when we log the `session` from useSession or getServerSession. 

This can be changes while using `calbacks` which we will see below.

Just writing above code, gives us automatically created `signin` page. 

That is, now we can access `/api/auth/signin`. And it will have a form with `username` and `password` that we provided above.

Next Step:

Create `providers.tsx` file, from here we will export the provider which whill wrap around the entire app, so that everyone has access to it's context.

And it will be a client component as we cannot use context inside a server component.

providers.tsx
```
"use client"

import { SessionProvider } from "next-auth/react";

export function Providers({children}) {
    return <SessionProvider>
                {children}
            </SessionProvider>
}

```

Now, in the root layout we will wrap the `children` with this provider.

```
import {signIn, signOut} from "next-auth/react"
```

we can directly use `signIn()` to go to the signIn page and `signOut()` to log out the user

### Accessing the Authenticated user's data

-   `useSession()` hook

    It is used to access the user's data inside client component

    ```
    "use client";

    import {useSession} from "next-auth/react"

    function AppBar() {
        const session = useSession()

        return <div> {JSON.stringify(session)} </div>
    }
    ```

-   `getServerSession()` function

    It is used to access the user's data inside server component

    ```
    import { getServerSession } from "next-auth"

    async function Home() {
        const session = await getServerSession()

        return <div> {JSON.stringify(session)} </div>
    }

    ```
---

### Callbacks in NextAuth

These are asynchronous functions whcih we can use to hook into certain step of authentications, like while signin, jwt creation, session creation.

This basically is helpful when we need to do extra operation in these steps.


-   #### SignIn callback

    ```
    const handler = NextAuth({
        providers: [],
        callbacks: {
            signIn: ({user}) {
                if(user.email === "malicioususer@gmail.com") 
                {

                    // don't allow user to authenticate

                    return false;
                }
                return true;
            }
        }
    })
    ```
    When using the `Credentials Provider` the `user` object is the response returned from the authorize callback and the profile object is the raw body of the HTTP POST submission.


- #### jwt callback

    This callback is called when the JWT is created (i.e, at signIn) or updated. The returned value will be encrypted and stored inside the cookie.

    Here we can add any extra thing in the token if we want or, we can manipulate the token
    
    We can persist data like user id here. When we return `id` from authorize function then it puts the `id` in the variable `sub`, we can verify it by logging the `token` in jwt callback

    ```
    async authorize(credentials, req) {

        return {
            id: 1,
            name: "Ritik",
            email: "raj@gmail.com"
        }
    },
    callbacks: {
            signIn: ({user}) {
                if(user.email === "malicioususer@gmail.com") 
                {
                    // don't allow user to authenticate
                    
                    return false;
                }
                return true;
            },
            jwt: ({token, user}) {
                token.userId = token.sub;

                return token;
            }
        }
    ```

- #### session callback

    This is called **whenever session is checked**. By default only a **subset of token is returned** for increased security. 

    To make something available that we added in the token (like user id) via jwt callback(), we have to **explicitly forward** it here to make it available to the client. This is the reason we could not see `id` when we logged the `session` from `useSession()`


    ```
    async authorize(credentials, req) {

        return {
            id: 1,
            name: "Ritik",
            email: "raj@gmail.com"
        }
    },
    callbacks: {
            signIn: ({user}) {
                if(user.email === "malicioususer@gmail.com") 
                {
                    // don't allow user to authenticate
                    
                    return false;
                }
                return true;
            },
            jwt: ({token, user}) {
                token.userId = token.sub;

                return token;
            }
            session: ({session, token, user}) {
                session.user.id = token.userID;

                return session;
            }
        }
    ```

    Now, after we provided id in the session callback,  we can see it in the client component when we access `session` from `useSession()` hook. But, we it's not available in the server component when we access it using `getServerSession()` function.

    Fo this, we need to provide NextAuth `authOptions` as an argument to the `getServerSession()` function. And then it will show the `id`

    **authOptions:** 
        
    It is basically the object that we put inside `NextAuth` above. (all the providers, authorze function, callbacks)

    Put this authOption into a different file and import it wherever it is required.


### Custom Signin Page

We can create signin route just like any route inside app,  and create a page there with our own custom signin page.

But still when we call `signIn()` taht we get from `next-auth/react` still it will take us to `/api/auth/signin`. To avoid this, we have add another key in `authOptions` called pages.

```
{
    providers: [],
    callbacks: {},
    pages: {
        signIn: "/signin"
    }
}
```

Now inside our custom `signin` page let's say we have abutton which authneticate using google, then we have to call `signIn("google")` when button is clicked. Basically, we have to call `signIn` function with the `provider` which we want to use.

I we want to use `credentials provider` then we will do:

```
signIn({"credentials", {
    name: "efwef",
    password: "ewrewgrg"
}})

```