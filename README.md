# devops-GHA-demo---MB
DevOpsJavaDemo - Project
This application was generated using JHipster 7.9.3, you can find documentation and help at https://www.jhipster.tech/documentation-archive/v7.9.3.

Project Structure
Node is required for generation and recommended for development. package.json is always generated for a better development experience with prettier, commit hooks, scripts and so on.

In the project root, JHipster generates configuration files for tools like git, prettier, eslint, husky, and others that are well known and you can find references in the web.

/src/* structure follows default Java structure.

.yo-rc.json - Yeoman configuration file JHipster configuration is stored in this file at generator-jhipster key. You may find generator-jhipster-* for specific blueprints configuration.

.yo-resolve (optional) - Yeoman conflict resolver Allows to use a specific action when conflicts are found skipping prompts for files that matches a pattern. Each line should match [pattern] [action] with pattern been a Minimatch pattern and action been one of skip (default if ommited) or force. Lines starting with # are considered comments and are ignored.

.jhipster/*.json - JHipster entity configuration files

npmw - wrapper to use locally installed npm. JHipster installs Node and npm locally using the build tool by default. This wrapper makes sure npm is installed locally and uses it avoiding some differences different versions can cause. By using ./npmw instead of the traditional npm you can configure a Node-less environment to develop or test your application.

/src/main/docker - Docker configurations for the application and services that the application depends on

Development
Before you can build this project, you must install and configure the following dependencies on your machine:

Node.js: We use Node to run a development web server and build the project. Depending on your system, you can install Node either from source or as a pre-packaged bundle.
After installing Node, you should be able to run the following command to install development tools. You will only need to run this command when dependencies change in package.json.

npm install
We use npm scripts and Webpack as our build system.

Run the following commands in two separate terminals to create a blissful development experience where your browser auto-refreshes when files change on your hard drive.

./mvnw
npm start
Npm is also used to manage CSS and JavaScript dependencies used in this application. You can upgrade dependencies by specifying a newer version in package.json. You can also run npm update and npm install to manage dependencies. Add the help flag on any command to see how you can use it. For example, npm help update.

The npm run command will list all of the scripts available to run for this project.

PWA Support
JHipster ships with PWA (Progressive Web App) support, and it's turned off by default. One of the main components of a PWA is a service worker.

The service worker initialization code is commented out by default. To enable it, uncomment the following code in src/main/webapp/index.html:

<script>
  if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register('./service-worker.js').then(function () {
      console.log('Service Worker Registered');
    });
  }
</script>
Note: Workbox powers JHipster's service worker. It dynamically generates the service-worker.js file.

Managing dependencies
For example, to add Leaflet library as a runtime dependency of your application, you would run following command:

npm install --save --save-exact leaflet
To benefit from TypeScript type definitions from DefinitelyTyped repository in development, you would run following command:

npm install --save-dev --save-exact @types/leaflet
Then you would import the JS and CSS files specified in library's installation instructions so that Webpack knows about them: Note: There are still a few other things remaining to do for Leaflet that we won't detail here.

For further instructions on how to develop with JHipster, have a look at Using JHipster in development.

JHipster Control Center
JHipster Control Center can help you manage and control your application(s). You can start a local control center server (accessible on http://localhost:7419) with:

docker-compose -f src/main/docker/jhipster-control-center.yml up
