# Steven Gutierrez | Portfolio Website

My personal portfolio website, built for class with Vue 3 and Vite. The design is based on a Japanese online shop, so each page works like a section of a store: the home page is the storefront, the About page is the shop owner profile, and the Portfolio page is the product listing.

## Pages

| Page | URL | What it shows |
| --- | --- | --- |
| Home | `/` | Intro, feature tiles, and a category strip |
| About | `/about` | My profile, school and work info, skills, and goals |
| Portfolio | `/portfolio` | My public GitHub repositories, loaded live |

## How the Portfolio page works

The Portfolio page lists my projects straight from GitHub, so I never have to update it by hand. When I make a new public repository, it shows up on the site automatically.

1. When the page opens, it sends a request to the GitHub API:
   `https://api.github.com/users/JahUnc/repos`
2. While it waits, the page shows a loading message.
3. When the data comes back, each repository is shown as a card with:
   - the repository name (links to GitHub)
   - the description, if it has one
   - the main programming language, if GitHub detects one
   - the date it was last updated
4. If the request fails, the page shows a friendly error message and a Try Again button.

GitHub allows about 60 requests per hour without an API key. To stay under that limit, the page saves the results in the browser for 10 minutes, so moving between pages does not send a new request every time.

## Built with

- [Vue 3](https://vuejs.org/) for the pages and components
- [Vue Router](https://router.vuejs.org/) for moving between pages
- [Vite](https://vite.dev/) for the dev server and build
- The [GitHub REST API](https://docs.github.com/en/rest) for the project list
- [Netlify](https://www.netlify.com/) for hosting

## Run it on your computer

You need [Node.js](https://nodejs.org/) version 20.19 or newer.

```bash
# 1. Download the code
git clone https://github.com/JahUnc/portfolio-website-steven-gutierrez.git
cd portfolio-website-steven-gutierrez

# 2. Install the packages
npm install

# 3. Start the dev server
npm run dev
```

Then open the link it prints in the terminal (usually http://localhost:5173).

## Other commands

| Command | What it does |
| --- | --- |
| `npm run dev` | Starts the site locally and reloads when you save a file |
| `npm run build` | Builds the final version of the site into the `dist` folder |
| `npm run preview` | Runs the built version locally so you can check it before deploying |

## Project layout

```
src/
  App.vue            Main layout: header, nav bar, current page, footer
  main.js            Starts the app and connects the router
  style.css          Site-wide colors, fonts, and base styles
  router/
    index.js         List of pages and their URLs
  views/             One file per page
    HomeView.vue
    AboutView.vue
    PortfolioView.vue
  components/        Pieces shared across pages
    NavBar.vue
    ScrollingHeader.vue
    HomeIntro.vue
    FeatureTiles.vue
    CategoryStrip.vue
    SiteFooter.vue
  assets/            Images
public/              Files copied as-is (favicon, icons)
```

## Changing the colors or fonts

All the main colors and fonts are set in one place at the top of `src/style.css`. Change a value there and it updates across the whole site.

## Deploying

The site is hosted on Netlify and connected to this GitHub repository. Pushing to the `main` branch starts a new deploy.

Netlify settings:

- Build command: `npm run build`
- Publish directory: `dist`

## Author

Steven Gutierrez
GitHub: [@JahUnc](https://github.com/JahUnc)
