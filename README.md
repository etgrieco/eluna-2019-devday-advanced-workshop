# ELUNA 2019 DevDay+: Advanced Development Workshop

[Presentation slide show on GoogleDocs](https://docs.google.com/presentation/d/e/2PACX-1vSIMn-AHtGWOSFagHjh9baMUu5Oi0l4wvbcDWlTmxdTBw0dsgqfK1N0u0CFv_PdwG6wo2Bi1M19EU3d/pub?start=false&loop=false&delayms=3000)

## Day 0 Preparation:

### Initial setup
1. Fork this repostory to your Github account
1. Download and Install [Docker Community Edition](https://www.docker.com/products/docker-engine) for your machine platform.
1. Clone this repository: `git clone https://github.com/[myuser]/eluna-2019-devday-advanced-workshop`.

### Pull Docker images

To speed up the setup of your docker image, you can pull prebuilt versions the images to your computer. This is highly recommended since docker images can take a long time to build, and we cannot guarrantee that these large images will be fast to download in the convention center.

```sh
docker pull quay.io/nyulibraries/eluna-2019-devday-advanced-workshop:latest
docker pull quay.io/nyulibraries/eluna-2019-devday-advanced-workshop:e2e
```

### Getting started in Docker
1. With the Docker daemon running, build the Docker image.
      ```sh
      docker-compose build
      ```
1. Run the container service
      ```sh
      # The 'up' command will deploy any dependent services,
      # and run with the service's ports enabled and mapped to the host.
      docker-compose up web
      ```
1. Navigate to `http://localhost:8003/primo-explore/search?vid=NYU` to see the basic VIEW up and running!

### Making changes using volumes

Notice that in `docker-compose.yml` there is a `volumes` section defined for the `web` service. Here, your local directory is being mounted as a 'volume' to the container at the specified directory:

```yml
services:
  web:
    #...
    volumes:
    - ./primo-explore/custom:/app/primo-explore/custom/
```

The gulp process will therefore dynamically respond to changes in the files in the local filesystem, which are reflected in the container and re-run there. To test that this is working, change some files and watch the page refresh and reflect those changes. For example:

`NYU/main.js`
```js
app.run(() => {
  console.log('hello from the main.js!');
});
```
`NYU/scss/main.scss`
```scss
p {
  color: red;
}
```

### Create a Primo frontend package

Now, creating a Primo frontend package is as simple. And Docker containers will help guarrantee that your output is not going to be effected by variations in environment!

```sh
VIEW=[VIEW] docker-compose run create-package
```

For now, the `VIEW` value can be either `NYU` or `CENTRAL_PACKAGE`. You will see the package output in the `packages` directory as `[VIEW].zip`.

### Other requirements

Install:
* Node (>= version 8)
* [Node Version Manager](https://github.com/creationix/nvm) for handling multiple versions of Node on your local machine (or, [nvm-windows](https://github.com/coreybutler/nvm-windows))
* [yarn](https://yarnpkg.com/lang/en/docs/install/) for node dependency management
* Google Chrome (latest version)
* Cypress:
```sh
yarn global add cypress
cypress verify
cypress open
```
* Create an account at [circleci.com](https://circleci.com) using your Github account.

### More resources
* [Adding an NPM dependency](https://github.com/NYULibraries/eluna-2019-devday-advanced-workshop/tree/master/.docs/add-npm.md)
