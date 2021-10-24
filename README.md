This is a starter template for [Learn Next.js](https://nextjs.org/learn).
24Oct21:https://nextjs.org/learn/seo/crawling-and-indexing/canonical
# Notes
1. client-side navigation
  - page transition using javascript which is faster than default navigation by browser
2. Code splitting
  - when a page is rendered, code for other pages are not served initially.
3. Pre-fetching
  - in production build, Link component will automatically <mark>prefetch</mark>for the linked page in background.
4. Pre-rendering
  - NextJs generates a pre-rendered HTML. After Javascript loads, hydration takes place.
  - Pure React does not render anything before javascript runs.
  - two forms of pre-rendering(Static Genaration or Server-Side Rendering)
    - Static generation (pre-rendering method that generates HTMl at build time and reused on each request)
    - server-side rendering (pre-rendering method that generates the HTML on each request)
    - development npm run build, every page is pre-rendered on each request, even for pages using static generation.
      - it follows that <mark>getStaticPaths</mark> in development runs on every request and in production runs at build time.
    - production, i think follows static generation/SSR respectively.
    - can create hybrid of pages of either static generation and server-side rendering
    - <mark>getStaticProps</mark> (for static generation) (fetch data at build time)
      - For static generation with data, use nextJs getStaticProps() to fetch external data at build time and send it as props to the page
      ```javascript
      //pages/index.js
        export async function getStaticProps() {
          const allPostsData = getSortedPostsData()
          return {
            props: {
              allPostsData
            }     
          }
        }

        export default function Home({allPostsData}) {
      ```
      - <mark>getStaticProps</mark> only runs on the serverside(since it runs at build time which builds in server)
      - <mark>getStaticProps</mark>  can only be exported from a page.
    - <mark>getServerSideProps</mark> (for server-side rendering) (for fetch data at request time instead of at build time)
      ```javascript
        export async function getServerSideProps(context) {
            return {
              props: {
                // props for your component
              }
            }
          }
      ```
  - Client-side Rendering(I think it is static generation with data but does not need to pre-render the data)
5. Dynamic Routing
  - <mark>getStaticPaths</mark> is like getStaticProps...it fetches dynamic ids for the purpose of [id].js file
  ```javascript
    export async function getStaticPaths() {
      const paths = getAllPostIds()
      return {
        paths,
        fallback: false
      }
    }
  ```
  - then in [id].js, <mark>getStaticProps({params})</mark> where params.id will refers to the id for the dynamic route to fetch the relevant id props for pre-rendering.

# Misc
1. css modules
  - layout.js 
    ```javascript
      import styles from "./layout.module.css"
      export default function Layout() {
        return (
          <div className={styled.container}> </div>
        )
      }
    ```
  - layout.module.css
    ```javascript
      .container {
      }
    ```
  - generate unique class names e.g. class="layout_container_xxx"
  - <mark>css modules</mark> imports css files into React component
  - <mark>css modules</mark> are extracted from javascript bundles at build time and generate .css files that are loaded by NextJs.
  - for global.css , create pages/_app.js and import in styles/global.css for the App component.
  ```javascript
    import "../styles/global.css"

    export default function App({ Component, pageProps }) {
      return <Component {...pageProps} />
    }
  ```

# Notsure

- fallback in getStaticPaths