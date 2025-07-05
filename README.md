# Supply Chain Carbon Calculator

A modern Vue.js application for calculating carbon footprints across supply chains. Built with Vue 3, Vite, and Tailwind CSS.

## Features

- 🌱 Carbon footprint calculation for supply chain components
- 📊 Interactive charts and visualizations
- 📄 PDF export functionality
- 🎨 Modern, responsive UI with Tailwind CSS
- 📱 Mobile-friendly design

## Recommended IDE Setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur).

## Customize configuration

See [Vite Configuration Reference](https://vite.dev/config/).

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```

### Lint with [ESLint](https://eslint.org/)

```sh
npm run lint
```

## Deployment

This project is configured for automatic deployment to GitHub Pages.

### Automatic Deployment

The application automatically deploys to GitHub Pages when changes are pushed to the main branch. The deployment process:

1. Builds the application using Vite
2. Deploys to GitHub Pages using GitHub Actions
3. Makes the site available at: `https://[your-username].github.io/supply-chain-carbon-calculator/`

### Manual Deployment

To manually trigger a deployment:

```sh
npm run deploy
```

Then push your changes to the main branch:

```sh
git add .
git commit -m "Update for deployment"
git push origin main
```

### GitHub Pages Setup

To enable GitHub Pages for your repository:

1. Go to your repository on GitHub
2. Navigate to Settings → Pages
3. Under "Source", select "GitHub Actions"
4. The workflow will automatically deploy your site

## Live Demo

Visit the live application: [Supply Chain Carbon Calculator](https://[your-username].github.io/supply-chain-carbon-calculator/)
