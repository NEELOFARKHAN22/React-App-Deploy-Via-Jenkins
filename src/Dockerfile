# Use Node 16 alpine as parent image
FROM node:16-alpine

# Change the working directory on the Docker image to /app
WORKDIR  /home/cloud_user/node_project/public


# Copy package.json and package-lock.json to the /app directory
COPY package.json package-lock.json ./

# Install dependencies
RUN npm install

# Copy the rest of project files into this image
COPY . .

EXPOSE 3000

CMD npm start

