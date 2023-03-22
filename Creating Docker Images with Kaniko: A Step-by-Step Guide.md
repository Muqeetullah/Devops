# Creating Docker Images with Kaniko: A Step-by-Step Guide
Docker has become a popular tool for building, packaging, and deploying applications in a containerized environment. However, one of the challenges of using Docker is the process of building images. The traditional method of building Docker images involves using a Dockerfile, which can be time-consuming and resource-intensive, especially for larger projects. Moreover, Dockerfile builds require access to the Docker daemon, which can pose security risks and limit its use in certain environments.

Fortunately, there is a solution to this problem: Kaniko. Kaniko is an open-source tool that enables users to build Docker images without needing access to the Docker daemon. This means that users can build Docker images from within a container, without requiring privileged access to the host machine. Additionally, Kaniko uses a cache mechanism to speed up the build process, reducing the time and resources required to build Docker images.

## Why Use Kaniko?
While Docker is an excellent tool for building container images, it has its downsides. Docker requires access to a Docker daemon, which can cause security and operational issues in multi-tenant environments.

Additionally, Docker can be challenging to use in some environments, such as those where Docker is not available or not desirable. Kaniko solves these problems by providing a Dockerfile-based container image-building tool that runs as an unprivileged user in a Kubernetes cluster or a Docker container.

## Prerequisites for Using Kaniko
Before we dive into building Docker images with Kaniko, there are a few prerequisites that you’ll need to meet:

#### - A Dockerfile: Kaniko builds container images using a Dockerfile, just like Docker. Make sure you have a Dockerfile that defines the build process for your image.
#### - A Kubernetes Cluster or Docker environment: Kaniko can run in a Kubernetes cluster or a Docker environment. Make sure you have access to a cluster or Docker environment where you want to build the container image.
#### -  Kaniko binary: You’ll need to download the Kaniko binary and make it executable on your local machine.
#### - Google Cloud Storage or Amazon S3 bucket (Optional): You can optionally store the built images in a Google Cloud Storage or Amazon S3 bucket. (Optional)
In this article, we will be focusing on building Containers with Kaniko in Docker Environment but first, let's see what are the benefits of running DockerFile in Kaniko

### Benefits of running Images in Kaniko
Kaniko allows you to build container images in a Kubernetes-native way, without relying on a Docker daemon. This eliminates the need for a Docker host, which can save time and resources. Moreover, Kaniko can be easily integrated with a CI/CD pipeline, which allows for seamless image building and deployment.

Kaniko also supports building images from non-privileged containers, which enhances security by reducing the attack surface of the container host. This is especially useful in multi-tenant environments where different users may have different security requirements.

Now, let’s dive into building a Docker image with Kaniko.

for this example, I will be using this Dockerfile, If you want to use the same file you can access it by cloning my git repository

##### Step 1: Create a Dockerfile: Create a Dockerfile that describes the components that should be included in the image, such as the base image, the software packages, and the configuration files.

My Docker file is

```
##### Base image
FROM node:16-alpine

##### Set the working directory
WORKDIR /app

##### Copy package.json and package-lock.json
COPY package.json ./

##### Install dependencies
RUN npm install --silent

##### Copy the app
COPY . ./

##### Build the app
RUN npm run build

##### Serve the app
CMD ["npm", "start"]
```
Now Run the following command
```
docker run --rm \
-v $(pwd):/workspace \
gcr.io/kaniko-project/executor:latest \
--context=/workspace \
--dockerfile=./path/to/Dockerfile \
--destination=<registry>/<repository>:<tag> \
--no-push=false \
--registry-mirror=https://registry-1.docker.io
```
In this command, you’ll need to replace the following:

`./path/to/Dockerfile`: The path to the Dockerfile relative to the current working directory ($(pwd)).
`<registry>`: The name of the container registry to push the image to (e.g. docker.io for Docker Hub).
`<repository>`: The name of the repository to push the image to.
`<tag>`: The tag to apply to the image.
When running Kaneko following three commands are must so lets look at them one by one

- Set the Build Context: Specify the build context by setting the` --context` flag followed by the root directory of the build context.
- Specify the Dockerfile: Specify the Dockerfile to be used for the build process by setting the `--dockerfile` flag followed by the path to the Dockerfile.
- Set the Destination: Set the destination for the built image by setting the `--destination` flag followed by the name and tag of the image.
Finally, your Image building is complete and now you can see the newly pushed image in your container registry

## Conclusion
Kaniko is a powerful tool that offers a more flexible, portable, and secure way to build Docker images without requiring a Docker daemon. In this article, we provided a step-by-step guide on how to use Kaniko to build Docker images and explained why it is a better alternative to Docker in some scenarios. With Kaniko, you can build container images faster and more securely, making it a valuable tool for developers and DevOps teams.
