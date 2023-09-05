FROM registry.access.redhat.com/ubi8/nodejs-18:latest as build-step

WORKDIR /opt/app-root
ENV PATH /opt/app-root/node_modules/.bin:$PATH

COPY --chown=default:root ./frontend/*.json ./
COPY --chown=default:root ./frontend/src ./src
COPY --chown=default:root ./frontend/public ./public

RUN whoami
RUN groups $(whoami)
RUN npm install
#RUN chown -R default:root node_modules && chmod -R 777 node_modules
RUN npm run build

FROM registry.access.redhat.com/ubi9/python-39:latest
WORKDIR /opt/app-root/src
#RUN mkdir -p frontend
COPY --from=build-step /opt/app-root/build ./frontend/build

# By default, listen on port 8081
EXPOSE 8081/tcp
ENV FLASK_PORT=8081

# Set the working directory in the container
#WORKDIR /projects

# Copy the dependencies file to the working directory
COPY requirements.txt .

# Install any dependencies
RUN pip install -r requirements.txt

# Copy the content of the local src directory to the working directory
COPY . .

# Specify the command to run on container start
CMD [ "python", "./app.py" ]
