# IBM Cloud Functions with Python or NodeJS and Docker

Examples of how to create actions in Python or NodeJS on the IBM Cloud Functions using Docker images.

## Getting Started

These instructions will help you to create a action on IBM Cloud Functions through Docker images.

### Prerequisites

What things you need and how to install them:

* [Docker](https://docs.docker.com/engine/installation/) - Docker images and containers
* [Docker Hub Account](https://hub.docker.com/) - Docker Hub is a cloud-based registry service which allows you to link to code repositories
* [NodeJS](https://nodejs.org/en/download/) - An asynchronous event driven JavaScript runtime
* [Python 3.6](https://www.python.org/downloads/) - Python is an interpreted, object-oriented and high-level programming language
* [IBM Cloud CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html#download_install) - IBM Cloud CLI provides the command line interface to manage applications, containers, infrastructures, services and other resources in IBM Cloud.
* [Cloud Functions CLI](https://console.bluemix.net/docs/openwhisk/bluemix_cli.html#cloudfunctions_cli) - Cloud Functions offers a powerful plug-in for the IBM Cloud CLI that allows complete management of the Cloud Functions system.

## Creating the Actions in IBM Cloud Functions

### Creating Python Actions

First you need create a Dockerfile
```
FROM openwhisk/python3action

# lapack-dev is available in community repo.
RUN echo "http://dl-4.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories

# add package build dependencies
RUN apk add --no-cache \
        g++ \
        lapack-dev \
        gfortran

# add the main file to image
ADD __main__.py ./

# add python packages
RUN pip install \
    watson-developer-cloud
```

Create your Python file with entry point
```
#__main__.py
from watson_developer_cloud import ConversationV1

def main(dict):
  greeting = "Hello Python with Docker!"
  return {"text": greeting}
```

Build your Docker image
```
docker build -t repository_name/python_image .
```

Push the Docker image to your Docker Hub
```
docker push repository_name/python_image
```

Create your Python action
```
wsk action create python_action --docker repository_name/python_image __main__.py
```

Calling your action

```
wsk action invoke python_action --result
```
