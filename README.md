# lume-build

Create a site with Lume

## Example usage:

The following GitHub workflow will deploy to pages, using the [deploy-pages](https://github.com/actions/deploy-pages) action.


```yaml
name: Build site

'on':
  workflow_dispatch: {}
  # schedule:
  #   # * is a special character in YAML so you have to quote this string
  #   - cron:  '45 23 * * *'
  push:
    branches: [ main ]

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Build the site
        uses: dringtech/lume-build@v2

  # Deploy job
  deploy:
    # Add a dependency to the build job
    needs: build

    # Grant GITHUB_TOKEN the permissions required to make a Pages deployment
    permissions:
      pages: write      # to deploy to Pages
      id-token: write   # to verify the deployment originates from an appropriate source

    # Deploy to the github-pages environment
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}

    # Specify runner + deployment step
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
``````