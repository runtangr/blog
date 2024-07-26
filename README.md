## It is easy to deploy site with github pages
1. Use workflows
https://github.com/runtangr/runtangr.github.io/blob/master/.github/workflows/deploy.yml
    * Setup Node.js
    * Install Hexo CLI
    * Install dependencies
    * Build for making static files
    * Deploy to GitHub Pages(use PERSONAL_TOKEN for pushing code to your branch 'gh-pages')
2. Use Github Pages
    * `pages build and deployment` action was set in `Setting Pages`
    * `pages build and deployment` actions will run when gh-pages branch recieve static files
    
