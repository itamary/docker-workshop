# Docker workshop

For this workshop we need DockerCE installed on your laptop/server

` docker --help `

Lets pull an image of nginx:
` docker pull alpine`

Let's verify this image is pulled:
` docker images `

Now - let's run it:
`docker run -it alpine`

Let's run some more interesting image:
` docker run -d -p 80:80 --name=nginx nginx:alpine`

Notice we ran it with `-d` so the docker is in detach mode and not attached to our current screen. In order to check the status of the docker container we can execute:
` docker ps`

Also we user `-p` option in order to map port 80 from the container to our host.
now we can browse into the server: http://localhost

We can see the logs of the container:
`docker logs <Container ID>`
We can use `-f` flag to follow the log.

If we would like to stop the container we will use the command:
`docker stop  <Container ID>`
or
`docker kill  <Container ID>`

After the container is stopped - we will not see it on
`docker ps`

in order to see all of the containers (started and stopped) we can use the `-a` flag
`docker ps -a`

to start stopped container we will use:
`docker start <Container ID>`

to remove stopped docker container - we will use:
`docker rm <Container ID>`

to remove running container we will use the `-f` flag:
`docker rm -f <Container ID>`

And let's start the nginx container again:
` docker run -d -p 80:80 --name=nginx nginx:alpine`

In order to execute command running container we will use:
`docker exec -it <Container ID> <command>` 
for example:
`docker exec -it nginx ls -l /`

We can use it to connect into container by running a shell:
`docker exec -it nginx sh`

Other flags of docker run are:
`-v` for mounting folder from the host to the container
`-e` for setting environtment variable

Example command:
`docker run -it -e TEST=myname -v ~/docker-workshop/src:/mnt alpine`
now we can run `env` and see the environment variable and see the content of mnt by `ls /mnt`

In order to do some checks we may would like to copy files from/to our container. We will use the command `docker cp` for example:
`docker cp src/index.html nginx:/usr/share/nginx/html`
We changed the default index file on our nginx container and now when we will check http://localhost we will see the new page

# Now let's build our own docker image:
For this exrecise we will need docker-hub account.

First docker image we will create will be a demo PHP site.
We copied a site from https://startbootstrap.com/themes/freelancer/ to the folder php-site. and created a simple Dockerfile.

In order to build the image we will run the command:
`docker build -t php-site:version1 .`

Our docker image is ready. to test it we can execute:
`docker run -d -p 80:80 php-site:version1`
And check that the site is working as expected on http://localhost

In order to tag our image - we can use the command `docker tag` for example:
`docker tag php-site:version1 <your-dockerhub-account>/php-site:latest`

we can save our new image in dockerhub - for that we need to login:
`docker login`

after login we can push the image:
`docker push <your-dockerhub-account>/php-site:latest`

We can continue playing and build/push more images.