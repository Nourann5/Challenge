## Dockerfile Overview
This CLIENT uses a multi-stage Dockerfile to build and serve the Nuxt.js frontend application efficiently. Below is a breakdown of the stages and techniques used in the Dockerfile:

### Stage 1: Build the Frontend
In the first stage, the application is built using Node.js. This stage installs dependencies and compiles the production-ready files.
#### Base Image: 
A slim version of Node.js 20.17.0 is used to minimize the image size.

#### Dependency Files: 
Only `package.json` and `package-lock.json` are copied initially to ensure that Docker uses caching for the dependency installation step if there are no changes in the files.

#### Install Dependencies: 
Installs the required dependencies using `npm install` and removes unnecessary packages with `npm prune` to optimize the build.

#### Build the Application: 
Compiles the application, usually generating production-ready files under a directory (e.g., `.output` for Nuxt.js).

### Stage 2: Serve the Frontend
In the second stage, the application is set up for serving in a production environment.
#### Separate Image: 
A second lightweight Node.js image is used for the deployment phase. This helps to exclude unnecessary development dependencies and keep the production image smaller.

#### Multi-Stage Copy: 
The build files generated in the first stage `/app/.output` are copied into the second stage for deployment. This ensures that only the necessary files are carried over to the final image.

#### Expose Port: 
The application exposes port `3000` for serving the frontend, making it accessible externally.

#### Run the Application: 
The application is served by running the `index.mjs` file inside the `.output/server` directory using Node.js.
