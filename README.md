# About

This a tutorial on how to dockerize a front-end application make with
React. You could probably easily adapt it to other frameworks.

# Prerequisites

You'll need to have node.js installed, with a working `npm`. You'll
also need to run the following commands into a terminal.

# Create a React app

```
npm install create-react-app
npx create-react-app myapp
```

This will create a `myapp` directory with your React project files.
This is also the directory that this repository hosts.

You can now enter that directory and do a test run of your app:
```
cd myapp
npm run start
```

Your React app should be visible at http://127.0.0.1:3000/

You can now shut it down (^C) so that we can go on with creating the
Docker container.

# Create a Docker container

Create a file named `Dockerfile`, see the respective file in this repo.
You can edit the node version that you'd like to use.

If you app needs a build step, append it to the RUN command. Here's an
example:
```
RUN npm i && \
  npm run build
```

Also create a `.dockerignore` file that lists all files/directories that
you don't want to include in the container image. See the respective
file in this repo. This will make speed up the build process and also
make the resulting container image smaller.

You can create a docker container that will launch your app as soon as
it is started with:
```
docker build -t myreactapp .
```

The `-t` option gives a tag (name) to your container and the `.` just
means to build whatever is in the present directory.

As soon as the `build` command finishes, you should be able to see your
container if you run:
```
docker images
```

# Start your container

You can start your dockerized app with:
```
docker run --name myreactapp -p 3000:3000 myreactapp
```

As the application is published through port 3000, with the `-p
3000:3000` part, you specify that you want this port to be accessible
from the host system. Otherwise it will only be available from *within*
the container, which may not be very useful.

If you don't provide a name with the `--name` option, docker will
assign a random one.

At this point your app should be available from your host system at
http://127.0.0.1:5001/

With this setup, you should be able to rebuild and run your dockerized
app in any system that supports docker without having to worry about
dependencies and different node or library versions.

# Stopping the container

To stop the container, just run:
```
docker stop myreactapp
```

If you didn't assign a specific name when you ran it, find out how the
running docker container is called first by running:
```
docker ps
```
which shows all running or stopped docker containers. The name is shown
in the last column.

You may then also delete the container if you run
```
docker rm myreactapp
```

# .gitignore file

(Optionally) add a .gitignore file to your git repo so that you don't
add the `venv` directory and anything else you might not want, like
byte-compiled python files in your git repo. See example in this repo.

