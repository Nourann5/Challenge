# Stage 1: Build the frontend
ARG NODE_VERSION=20.17.0
FROM node:${NODE_VERSION}-slim AS build

# Set working directory inside the container
WORKDIR /app

# Copy the package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install
RUN npm prune

# Copy the rest of the application files
COPY . .

# Build the application
RUN npm run build

# Stage 2: Serve the frontend
FROM node:${NODE_VERSION}-slim AS deploy
#Specify the work directory
WORKDIR /app 
# Copy the build files from the previous stage to the NGINX web server directory
COPY --from=build /app/.output  /app/.output 

ARG PORT=3000
ENV PORT=$PORT

EXPOSE 3000
CMD [ "node", ".output/server/index.mjs" ]
