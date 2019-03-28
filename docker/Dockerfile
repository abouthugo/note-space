# Example for a node application

FROM node:latest

# Stablish working directory and cd into it
WORKDIR /usr/src/app

# Copy the package.json file into current directory (Yes, WORKDIR)
COPY package*.json ./

# Install dependencies
RUN npm Install

# Copy source code (you must define .dockerignore to ignore the node_modules directory)
COPY . . 

# Container's port on which this can be accessed through 
EXPOSE 8080

# Run the image 
CMD ["npm", "start"]