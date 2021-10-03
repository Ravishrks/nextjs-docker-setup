# Next js development with Docker and wsl2

This article will guide you to setup Next js development enviroment with Docker.
If you are begenniner. You will have to deal with various bugs and may take a lot to have successful setup on Windows machine.

At the end of this article. You are ready to run any Next js based projects using Docker container with wsl2 support.

## Problems on Windows

Next js uses hot reloading features to update the browser on file change events. If you have used Docker setup earlier on Windows you may see issue like.

* Browser is not refreshing when you have changes on file.

* You may have permission issues on files.

This project will make sure to save your time setting up Docker on Windows for Development enviroment with Next js.

## Why WSL2 backend requirements with this project?

This project uses hot reloading features to sysnc UI change. Which refreshes your browser whenever there is changes in file. And Docker is not able to listen for file change event without WSL2 support.

If you are new to WSL2 and Docker, kindly check out this article to get some context about using WSL2 with Docker. After going through this article, you will be comfortable running any Docker based development project on Windows machine.

## Understanding file structure

Below is the file structure which you can get fromn the github repository.

```sh
app     (Next js project folder)
dev     (Docker developement enviroment setup folder)
    Dockerfile
    docker-entrypoint.sh
docker-compose.yml     (Compose file to run services)

```

### Docker file content

```Dockerfile
FROM node:latest

EXPOSE 3000

USER root

COPY docker-entrypoint.sh /usr/local/bin/
RUN chmod 777 /usr/local/bin/docker-entrypoint.sh
ENTRYPOINT ["docker-entrypoint.sh"]
    
WORKDIR /workspace

CMD [ "node" ]

```

### Docker compose file content

```yml
version: "3.9"
services:
  devenv:
    build: ./dev
    ports:
      - "3000:3000"
      
    volumes: 
      - .:/workspace

    command: /bin/sh -c "cd /workspace/app; yarn dev"
```

## Open project in WSL2 container using your favorite Linux distro (say Ubuntu)

Open WSL2 filesystem. In case you are unfamilear with WSL2 filesystem, pleae go through below article and come back later to thi step.

### Git clone the repository in Linux filesystem

Use below command to clone this repository

```sh
git clone https://github.com/Dblogstream/nextjs-docker-setup.git
```

### Go into project folder

Go to project folder and open VS code studio. It may take some time to download VS code server if you are doing it for the first time.

Below command will go to project folder and open code editor

```sh
cd nextjs-docker-setup && code .
```

### Start docker container

It will open contsiner shell for you, and you can create your next project using this shell.

```sh
docker-compose run -p 3000:3000 -p 80:80 devenv /bin/bash
```

## Creating Next js project

I suggest you to visit Nuxt js official docimentation to do rest of the things. You can follow here as well.

### Create Next app

Run below command and follow on screen instructions.

```sh
yarn create next-app 
```

If you want TypeScript support.

```sh
yarn create next-app --typescript
```

## Start development server

Close all the process pressing (Crtl key + C or type 'exit'). And use Docker compose to have development enviroment. Enjoy development. You will not face any issue developing Next projects.

```sh
docker-compose up
```

### Stop development server

```sh
Crtl key + C
```

### Need to run shell command

Open another terminal and run Docker container without specifing ports.

A new shell will open that will let you run any commands as you are on Linux Machine.

```sh
docker-compose run devenv /bin/bash
```

## See changes on file change events

Modify index page. You will see changes in real time. Happy coding.
...

## Conclusion

You have to use wsl2 backend with Docker for fast as well as expected Linux filesystem behaviour.

In case you have trouble in any steps. Feel free to contact me. I am more than happy to help you.

I strongly request you to share your experiances and issues. I will surely look into it and maybe find some solutions togethere.
