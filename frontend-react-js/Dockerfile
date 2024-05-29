# Specifies the base image to use, in this case, the official Node.js 16.18 image.
FROM node:16.18

# Sets the environment variable PORT to 3000.
ENV PORT=3000

# Copies all files from the host's current directory to the /frontend-react-js directory inside the container.
COPY . /frontend-react-js

# Sets the working directory inside the container to /frontend-react-js.
WORKDIR /frontend-react-js

# Runs npm install command to install dependencies defined in package.json.
RUN npm install

# Exposes port 3000 (specified by the PORT environment variable) for incoming connections.
EXPOSE ${PORT}

# Specifies the default command to run when the container starts, which is npm start. 
# This command typically starts the application defined in the package.json file.
CMD ["npm", "start"]