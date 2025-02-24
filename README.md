**Deploye Config File for Vite+React**

Step :1
-------

* Goto the vite config file
	add this comment
	import { defineConfig } from 'vite'
	import react from '@vitejs/plugin-react'

	// https://vitejs.dev/config/
	export default defineConfig({
 	plugins: [react()],
  	base:"/your repository name/"  --------->Enter you repository name
	})

Step :2
-------

* Create a deploy folder "./project/created folder"

	---> Create a workflow folder in the above node module folder. 

	---> Create a folder and named it ".github".

	---> Then Create a folder with in the ".github" folder and named it "workflows".

		like this : .github/workflows
	
	---> Next Create a file into "workflows" folder and named it "deploy.yml"
	
		like this : .github/workflows/deploy.yml

Step :3
-------

* Paste the give code into the "deploy.yml" file.

--------------------/ Copy this code and paset in deploy.yml file /--------------------

name: Deploy

on:
  push:
    branches:
      - main

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v4 # Updated to v4

      - name: Setup Node
        uses: actions/setup-node@v4 # Updated to v4
        with:
          node-version: '20' # Specify the Node.js version you need

      - name: Install dependencies
        uses: bahmutov/npm-install@v1

      - name: Build project
        run: npm run build

      - name: Upload production-ready build files
        uses: actions/upload-artifact@v4 # Updated to v4
        with:
          name: production-files
          path: ./dist

  deploy:
    name: Deploy
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
      - name: Download artifact
        uses: actions/download-artifact@v4 # Updated to v4
        with:
          name: production-files
          path: ./dist

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist

    steps:
      - name: Download artifact
        uses: actions/download-artifact@v3
        with:
          name: production-files
          path: ./dist

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist

--------------------/ Code ended /--------------------

Step :3
-------

* This code into your github repository
	
	* enter this commend in your terminal
		
		--> git add .
		--> git commit -m "Deploy file configed"
		--> git push
Step :4
-------

* On your repo page:

	--> Go to Settings → Actions → General,
	--> Scroll down to Workflow Permissions,
	--> Choose Read and Write, and save:

Step :5
-------

* Re-run actions :

	--> Actions → choose a failed deployment → Re-run failed jobs. Wait until in finishes.

Step :6
-------

* Configure GitHub Pages:

	--> Go to Settings → Pages
	--> Under Source, choose "Deploy from branch" and select the "gh-pages" branch.
        --> Click Save.

* Once the workflow finishes successfully, you'll see a new deployment on your GitHub Pages. Click the link to access your deployed Vite React application!.
