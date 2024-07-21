# Week 1 — App Containerization

The goal this week seems to be simple: containerize the backend and the frontend. 

However, in order to get there I first have to make sure that everything works fine separately before using `docker compose`.

Therefore, the activities to be executed follow.

## Containerize the backend

A good practice is to run from the command line the commands that are included in the `Dockerfile`, to make sure that everything results as expected.

Since my development environment is Gitpod, I first make sure that Python is already installed so that I can run the preceding commands.

Then, I need to install `flask`y `flask-cors` ([see](https://pypi.org/project/Flask-Cors/)).

So, from the command line type:
```sh
cd backend-flask
export FRONTEND_URL = "*"
export BACKEND_URL = "*"
python3 -m flask run --host = 0.0.0.0 --port=4567
cd ..
```
After appending an API endpoint to the URL, namely `api/activities/home`,I expect to see the a .json file with some mock-up text.

Now that I everything seems to be working as expected, I am going to create the Docker container. 

### Add the Dockerfile

Since the commands used for deploying the backend are working successfully , I am confident that the `Dockerfile` ([read file](https://github.com/rodvel-inc/aws-bootcamp-cruddur-2023/blob/main/backend-flask/Dockerfile)) will be just as efficient.

### Run the container
- The backend container needs the environment variables `BACKEND_URL` and `FRONTEND_URL` to be passed to it, in order to run correctly. Adding the `-e`flag to the `docker run`command helps me achieve this goal.
- The container will be short-lived, meaning that I will only use it while I test the container configuration is correct. Adding the `--rm` to the `docker run`command accomplishes this.
- I want to access to the container's bash shell. Adding the `-it` option gets me there.

So, the final command will be:

```sh
cd backend-flask
docker run --rm -p 4567:4567 -it -e FRONTEND_URL='*' -e BACKEND_URL='*' backend-flask
```
## Containerize the frontend

Adhering to the same principle used in the backend section, I now will test everyhting from the command line before creating the frontend's `Dockerfile`.

The very first thing to do is to install NPM in the host machine. The reason is that the container needs to copy the contents of `node_modules` from this machine. Some people would argue this is a good practice if I want to keep my container as light as possible. So, from the command line in the host machine:

```sh
cd frontend-react-js
npm i
```
However, a way to make this permanent, instead of executing manually these 2 commands everytime a Gitpod workspace is launched is to add a section with the commands in the `.gitpod.yml`file, as shown here:

```yml
- name: react-js
    command: |
      cd frontend-react-js
      npm i
```
### Add the Dockerfile

The `Dockerfile` ([read file](https://github.com/rodvel-inc/aws-bootcamp-cruddur-2023/blob/main/frontend-react-js/Dockerfile)) is expected to work fine.

### Run the container

```sh
cd frontend-react-js
docker run --rm -p 3000:3000 -it frontend-react-js
```
After running this container, I expect to see the mock-up frontend with some hard-coded content.

## Containerize everything at once

At this point, the two containers have been tested and they do as expected. However possible, running these two containers separately is not a good idea. 

Enter docker compose. ([see](https://docs.docker.com/compose/)).

I first have to add a `docker-compose.yml`file to the project's root directory.

Of **utmost relevance** in this file is the way the **environment variables** are defined, so that they can be used by both containers when needed.

This is how the backend knows about it:

```yaml
version: "3.8"
services:
  backend-flask:
    environment:
      FRONTEND_URL: "https://3000-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
      BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
    build: ./backend-flask
this file continues...
```

This is how the frontend knows about it:

```yaml
frontend-react-js:
    environment:
      REACT_APP_BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
    build: ./frontend-react-js
    ports:
      - "3000:3000"
this file continues...
```
Finally, execute the corresponding command from the project's root directory:

```sh
docker compose -f "docker-compose.yml" up -d --build
```
## Where are my databases?

The application that I am trying to deploy consists of three components: frontend, backend and databases. Yes, databases in plural. 

See the deployment diagram in Week 0's documentation [here](https://lucid.app/lucidchart/c333b586-db78-4a7d-b6dd-8f22d17a8c83/edit?viewport_loc=-533%2C-66%2C4992%2C2343%2CV3dxByeoB0H6&invitationId=inv_fe3cd5fc-e7fa-44f8-ae98-850c76912554).


