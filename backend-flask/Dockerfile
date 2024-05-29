# This line specifies the base image to use for the container. 
# Here, it uses the official Python 3.10 slim image, a smaller version of the standard Python image, based on Debian Buster.
FROM python:3.10-slim-buster

# This sets the working directory inside the container to /backend-flask. 
# All subsequent commands will be run from this directory.
WORKDIR /backend-flask

# This copies the requirements.txt file from the host machine to the container's working directory.
COPY requirements.txt requirements.txt

# This installs the Python dependencies specified in requirements.txt using pip.
RUN pip3 install -r requirements.txt

# This copies all the files from the current directory on the host machine to the current working directory in the container. 
# Essentially, it copies your entire project into the container.
COPY . .

# This sets an environment variable FLASK_ENV to development within the container, which configures Flask to run in 
# development mode (enabling debugging features).
ENV FLASK_ENV=development

# This informs Docker that the container will listen on the port specified by the PORT environment variable. 
# The actual value of PORT is provided at runtime. 
# This line is for documentation purposes and does not actually publish the port.
EXPOSE ${PORT}

# This specifies the command to run when the container starts. 
# It runs the Flask development server using Python's -m flask run command, listening on all network interfaces 
# (0.0.0.0) and on port 4567.
CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]