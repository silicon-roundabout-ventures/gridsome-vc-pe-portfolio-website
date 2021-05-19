# VC (or PE) portfolio JAMstack website

## Overview

TBD

## Stack used
 * Gridsome
 * TailwindCSS
 *

### Gridsome infrastructure
Gridsome stores`.vue` components in the `src/pages` directory create page routes. `gridsome build` generates static files in a `/dist` folder. [Here](https://gridsome.org/docs/core-concepts/) you can find the key concepts behind the gridsome architecture.

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

...
