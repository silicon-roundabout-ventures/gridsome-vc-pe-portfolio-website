# VC (or PE) portfolio JAMstack website

## Overview

TBD

## Stack used
 * Gridsome
 * TailwindCSS
 * Contentful

## Instructions
1. If you use `npm`, install the packages with `install npm`
2. Register and Create a space on Contentful (to rapidly test, I'd suggest to use the "Blog" example when creating a new space)
3. Create a `.env` file to store your own API keys from Contentful (`Settings >> API Keys`) *(If you're not sure how to do this, see how I did it in the development log below)*
4. Run `gridsome develop` to run your local server
5. Your site should be live at http://localhost:8080)


### Gridsome infrastructure
Gridsome stores`.vue` components in the `src/pages` directory create page routes. `gridsome build` generates static files in a `/dist` folder. [Here](https://gridsome.org/docs/core-concepts/) you can find the key concepts behind the gridsome architecture. All the settings are defined in [`gridsome.config.js`](https://gridsome.org/docs/config/)

### Tools used

* text editor (Atom)
* terminal
* npm

### Project setup steps (dev log)

#### Project basic setup
1. Gridsome installed globally: `npm install --global @gridsome/cli`
2. `gridsome create <webste-project-name>` to install default starter
2. `cd <website-project-name>` to open the folder
3. `gridsome develop` to start a local dev server at `http://localhost:8080`
4. Initialise git `git init` and added all the files `git add .`
5. Commit: `git commit -m "first commit"`
6. Make main branch (I called it "main" but you can call it "master" or whatever else): `git branch -M main`
7. (Optional -- only if you want to link it to an online repo) Link online repository: `git remote add origin <Online-empty-repo-URL>`
8. (Optional) Push online: `git push -u origin main`
9. Test: in terminal execute `gridsome develop` -> then head to https://localhost:8080

#### Page Creation
1. The root of the project is where our main configuration file `gridsome.config.js` resides. `Index.vue` will be our homepage. Pages are usually used for normal pages or for listing items from a GraphQL collection.
Add .vue files here to create pages. For example **About.vue** will be **site.com/about**. Learn more about pages: https://gridsome.org/docs/pages/
2. To test this, try editing src/pages/About.vue:
    ```
    <template>
      <Layout>
        <h1>About us</h1>
        <p>I'm building a portfolio website with Gridsome.</p>
      </Layout>
    </template>

    <script>
    export default {
      metaInfo: {
        title: 'This is my About Page'
      }
    }
    </script>
    ```
3. Gridsome should rebuild automatically once you save the file and you should see the changes in https://localhost:8080/about
4. Add another page: eg. make a document in src/pages called `Faq.vue` and add the following HTML:
    ```
    <template>
      <Layout>
        <h1>FAQ</h1>
        <p>What's the meaning of life?</p>
      </Layout>
    </template>

    <script>
    export default {
      metaInfo: {
        title: 'This is the FAQ page'
      }
    }
    </script>
    ```
5. Again, you can see the results by typing https://localhost:8080/faq
6. Let's head to `layouts/Default.vue` and add the following after the `/about` link:
  ```
  <g-link class="nav__link" to="/faq">FAQ</g-link>
  ```


#### Build static pages
1. Use `gridsome build` to generate static files ─these will go to the /dist folder


#### Install TailwindCSS
1. Gridsome can [integrate with TailwindCSS](https://gridsome.org/plugins/gridsome-plugin-tailwindcss): `npm install -D gridsome-plugin-tailwindcss tailwindcss@latest` and `npm install -D postcss-import postcss-preset-env #(if you want these plugins and you've updated the config)`, then `npx tailwind init` to generate the `tailwind.config.js` file
2. Create a new folder under src called /assets/css and a file in the css folder called `global.css.` In `global.css add` type the tailwind default setup:
  ```
  @tailwind base;
  @tailwind components;
  @tailwind utilities;
  ```
3. Import the .css file to our src/main.js file by adding the line: `import "./assets/css/global.css";`
4. Setup the tailwind gridsome plugin in `gridsome.config.js`:
    ```
    plugins: [
      {
        use: "gridsome-plugin-tailwindcss",
        /**
        * These are the default options.

        options: {
          tailwindConfig: './tailwind.config.js',
          presetEnvConfig: {},
          shouldImport: false,
          shouldTimeTravel: false
        }
        */
      },
    ],
    ```
5. **NOTE!**: Test that gridsome can build fine. If it fails, your problem might be PostCSS compatibility. 🚨 If you get such errors, Tailwind has created a [compatibility version](https://tailwindcss.com/docs/installation#post-css-7-compatibility-build). This means: (a) remove broken packages (`npm uninstall tailwindcss postcss autoprefixer`) and (b)_reinstall the compatible verion (`npm install tailwindcss@npm:@tailwindcss/postcss7-compat postcss@^7 autoprefixer@^9 --force`). Once the rest of your tools have added support for PostCSS 8, you will be able to move off of the compatibility build by [re-installing Tailwind and its peer-dependencies using the latest tag](https://tailwindcss.com/docs/installation#post-css-7-compatibility-build).

#### Link Contentful

1. `add @gridsome/source-contentful markdown-it`
2. create in root a `.env` file to store your Contentful access keys ([environmental variables](https://gridsome.org/docs/environment-variables/))
  ```
  CONTENTFUL_SPACE="<YOUR-API-SPACE-KEY>"
  CONTENTFUL_TOKEN="<YOUR-API-TOKEN-KEY>"
  CONTENTFUL_ENVIRONMENT="master"

  ```
3. Add the Contentful plugin to the plugin array in ``:
  ```
  {
    use: '@gridsome/source-contentful',
    options: {
      space: process.env.CONTENTFUL_SPACE,
      accessToken: process.env.CONTENTFUL_TOKEN,
      host: 'cdn.contentful.com',
      environment: process.env.CONTENTFUL_ENVIRONMENT,
      typeName: 'Contentful'
    }
  }
  ```
  And the following after the plugin array to define the templates:
  ```
  templates: {
    ContentfulBlogPost: '/blog/:slug'
  }
  ```
  So that the final config setup looks like:
  ```
  module.exports = {
    siteName: 'Gridsome',
    plugins: [
      {...},
      {...}
    ],
    templates: {...}
  }
  ```
4. By default Gridsome is already setup to read data from our `.env` file. So we can immediately build a page where we can list all of our blog posts. Let's create a file `pages/Blog.vue` and add this code snippet below:
  ```
  <template>
    <Layout>
      <section v-if="$page">
        <ul>
          <li v-for="{ node } in $page.posts.edges" :key="node.id">
            <h2>
              <g-link :to="node.path">{{ node.title }}</g-link>
            </h2>
            <div>
              <span>{{ node.date }}</span>
            </div>
            <div>
              {{ node.excerpt }}
            </div>
            <div>
              <g-link :to="node.path">Read More</g-link>
            </div>
          </li>
        </ul>
        <pager
          v-if="$page.posts.pageInfo.totalPages > 1"
          :info="$page.posts.pageInfo"
        />
      </section>
    </Layout>
  </template>

  <page-query>
    query Posts($page: Int) {
      posts: allContentfulBlogPost( sortBy: "date", order:DESC, perPage: 3, page: $page ) @paginate {
        totalCount pageInfo {
          totalPages
          currentPage
        } edges {
          node {
            id
            title
            path
            date(format:"MMMM D, Y")
          }
        }
      }
    }
  </page-query>

  <script>
    import { Pager } from 'gridsome'

    export default {
      metaInfo: {
        title: 'Blog',
      },
      components: {
        Pager,
      },
    }
  </script>
  ```
5. Let's head to `layouts/Default.vue` and add the following after the `/faq` link:
  ```
  <g-link class="nav__link" to="/blog">Blog</g-link>
  ```
6. Create a template for our contentful blog post. Create a file in `templates/` called `ContentfulBlogPost.vue` (*The filename must match the collection name in the GraphQL environment. If you entered a different typeName in the gridsome.config.js for the contentful plugin. For example, ContentfulDataPost.vue if you chose contentfulDataPost as the typeName.*):
  ```
  <template>
    <Layout>
      <div>
        <h1>
          {{ $page.post.title }}
        </h1>
        <div v-html="content" />
      </div>
    </Layout>
  </template>

  <page-query>
    query Post($path: String!) {
      post: contentfulBlogPost(path: $path) {
        id,
        title,
        body,
        date (format: "MMMM DD, YYYY"),
        path
      }
    }
  </page-query>

  <script>
  import MarkdownIt from 'markdown-it'

  export default {
    metaInfo() {
      return {
        title: this.$page.post.title,
      }
    },
    computed: {
      content() {
        const md = new MarkdownIt()

        return md.render(this.$page.post.body)
      },
    },
  }
  </script>
  ```
