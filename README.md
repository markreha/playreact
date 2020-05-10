# A simple React that can be used for testing in various Cloud Platforms like Azure and Heroku. See the following directions for deploying a React application to Azure, AWS, Heroku, and Google Cloud.
This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 7.3.6.

## Available Scripts
In the project directory, you can run:
  * `npm start`
  * Runs the app in the development mode.
  * Open [http://localhost:3000](http://localhost:3000) to view it in the browser.
  * The page will reload if you make edits.

## Building and Testing
1. Run `npm run build` to build the project. The build artifacts will be stored in the `build/` directory.
2. Test the React App locally by using MAMP: Copy all the files under the buld directory to the MAMP htdocs/[APP_NAME] directory

## Deployment to Cloud Platform Instructions
## Azure:
1. Create a new Web App (if new application)
	1. Select the + Create a new Resource menu option.
	2. Select Web App, select to Publish Code, select Node 10.x Runtime stack either Windows (required for Zip deploy) or Linux, and select a desired Region. Click Review + Create. Click Create.
	3. After your new application deployment is finished click the application link to test out. Click go to Resource.
2. Open the Web App from the Dashboard
3. Deploy from a Build:
	1. Select Advanced Tools and click the Go link
	2. Select the Debug console->CMD menu options.
	3. Navigate to the site/wwwroot directory.
	4. Delete the default content.
	5. Zip up all the files within the build directory. 
	6. Drag and drop the zip file onto the right side of the CMD window.		 
4. Deploy from a GIT CI/CD Build Pipeline:
	1. Select the Deployment Center menu option.
	2. Select the GitHub CI/CD type (authorize access if necessary) and click the Continue button.
	3. Select the GitHub Actions type and click the Continue button.
		* Fill in the GitHub Repository
		* Select the master branch
		* Select the Node.JS Runtime stack
		* Select the Node Version
		* Click the Finish button.
		* In the GitHub repo modify the GitHub build workflow file in the .guthub/workflows directory
			* Change the package: . entry to package: ./build

## Heroku:
1. Create a new Web App (if new application)
    1. Select the New button and select the Create new app menu option. Give your app a Name. Click the Create Appp button.
    2. Select the GitHub deployment method and connection to your GitHub repository.
    3. After your new application deployment is finished click the application link to test out. Click go to Resource.
2. Open the Web App from the Dashboard
3. Configure the application:
    1. Update package.json to include the express library in the list of dependencies specifiying the version of express used in development:
        * "express": "^4.17.1"
    2. Update package.json to include a Heroku post install step to build the project in the list of scripts (this step is only required for CI/CD builds): 
		  * "heroku-postbuild": "react-scripts build"
	  3. Add a new file named Procfile to the repository with the following entry:
		  * web: node server.js
	  4. Add a new file named server.js to the repository that will be used to serve up the React application:
		  * Set the following code to initialize the Express application:
		  	* app.use(express.static(__dirname);
			* app.use(express.static(__dirname + 'build');
		  * The /route should contain the following code:
			* res.sendFile(path.join(__dirname, 'build', 'index.html');  
4. Deploy from a Build:
    1. Run a build using the npm run build command.
    2. Push all the code including the build directory to the repository.
    3. Select the Deploy menu from Heroku. Click the Deploy Branch button.
5. Deploy from a GIT CI/CD Build Pipeline:
    1. Push all the code excluding the build directory to the repository.
    2. Select the Deploy menu from Heroku. Click the Enable Automatic Deploy button. Click the Deploy Branch button.

## AWS:
1. Log into AWS and select Elastic Beanstalk from the Compute Services.
2. Create a new Web App (if new application)
	1. Click the Create a new application button.
	2. Give your application a name. Click the Create button.
	3. Click the Create a new environment button.
		* Web server environment
		* Platform of Node.js
		* Platform branch of Node.js 10 running on 64bit Linux
		* Sample application
		* Click the Create button
		* Once the environment is up and running test the Sample application to ensure all is working properly
3. Configure the application:
	1. Update package.json to include the express library in the list of dependencies specifiying the version of express used in development:
		* "express": "^4.17.1"
	2. Update package.json to include a Heroku post install step to build the project in the list of scripts (this step is only required for CI/CD builds): 
		* "heroku-postbuild": "react-scripts build"
	3. Add a new file named Procfile to the repository with the following entry:
		* web: node server.js
	4. Add a new file named server.js to the repository that will be used to serve up the React application:
		* Set the following code to initialize the Express application:
			* app.use(express.static(__dirname);
			* app.use(express.static(path.join(__dirname, 'build');
		* The /route should contain the following code:
			* res.sendFile(path.join(__dirname, 'build', 'index.html');  
4. Deploy from a Build:
	1. Run a build using the npm run build command.
	2. Zip up all the code and files within the build directory but do not include the node_modules and src directories.
	3. Click on the Upload and deploy button.
		* Click the Choose file button and select your zip file.
		* Click the Deploy button.
5. Deploy from a GIT CI/CD Build Pipeline:
	1. Configure code and setup build pipeline (if not already completed):
		* Add a buildspec.yml to the root of your application code for Node 10 application.
```yaml
		version: 0.2
		# Build the code
		phases:
		  install:
		    runtime-versions:
		      nodejs: 10  
		  pre_build:
		    commands:
		      - echo Installing source NPM dependencies...
		      - npm install
		  build:
		    commands:
		      - echo Build started on `date`
		      - npm run build
		  post_build:
		    commands:
		      - echo Build completed on `date`

		# Include only the files required for your application to run.
		artifacts:
		  files:
		    - server.js
		    - Procfile
		    - package.json
		    - build/**/*
		    - public/**/*
```

* Log into AWS and select Services from the main menu.
		* Select the CodePipeline service.
		* Click the Create Pipeline button.
		* Give your pipeline a name (i.e. TestAppPipeline). Click the Next step button.
		* Select GitHub from the Source provider dropdown. Click the Connect to GitHub button and select your repository and master branch. Click the Next step button.
		* Select AWS CodeBuild from the Build provider dropdown. Select the Create a new build project option. Give your build a name. Select Linux operating system with defaults and using a buildspec.
		* Click the Create Project button.
		* Click the Next step button.
		* Select AWS Elastic Beanstalk from the Deployment provider dropdown. Choose your Application and Environment from the dropdowns. Click the Next step button.
		* Click the Create pipeline button.
	2.	To build and deploy your application:
		* Select the CodePipeline service from the Services dashboard. Open the Pipeline.
		* Either make a change to code in GitHub or click the Release change button to start a build and deployment.
