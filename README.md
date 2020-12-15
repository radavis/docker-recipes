# Docker Recipes

Install docker. Clone this repository.

## Run Python code in a container

[python](https://hub.docker.com/_/python) docker image on dockerhub.

```bash
$ curl --remote-name https://learnxinyminutes.com/docs/files/learnpython.py
$ docker run -it \
    --rm \
    --name my-running-script \
    --volume "$(pwd)":/usr/src/myapp \
    --workdir /usr/src/myapp \
    python:3 \
    python learnpython.py

# or
$ ./bin/python3 learnpython.py
```

## Try out Ruby v3 features

[ruby](https://hub.docker.com/_/ruby) docker image on dockerhub.

```ruby
# ruby3-features.rb

# rightward assignment
rand(10) => x
puts "rand(10) is #{x}"

# endless method definition
def square(x) = x * x
puts "5^2 is #{square(5)}"
```

```bash
$ docker run -it \
    --rm \
    --name my-running-script \
    --volume "$(pwd)":/usr/src/myapp \
    --workdir /usr/src/myapp \
    ruby:3.0-rc \
    ruby ruby3-features.rb

# or
$ ./bin/ruby3 ruby3-features.rb
```

## Docker: bind mounts

> When you use a bind mount, a file or directory on the host machine is mounted
> into a container. The file or directory is referenced by its absolute path on
> the host machine. By contrast, when you use a volume, a new directory is 
> created within Docker’s storage directory on the host machine, and Docker 
> manages that directory’s contents. -- [https://docs.docker.com/storage/bind-mounts/]()

## Host static content with nginx

[nginx](https://hub.docker.com/_/nginx) docker image on dockerhub.

```bash
$ curl --location --remote-name https://github.com/h5bp/html5-boilerplate/releases/download/v8.0.0/html5-boilerplate_v8.0.0.zip
$ unzip -d html html5-boilerplate_v8.0.0.zip

# mount volume in read-only mode
$ docker run -d \
    --name nginx-with-volume \
    --volume "$(pwd)"/html:/usr/share/nginx/html:ro \
    --publish 8080:80 \
    nginx:stable

# bind mount the `html` directory
$ docker run -d \
    --name nginx-with-bind-mount \
    --mount type=bind,source="$(pwd)"/html,target=/usr/share/nginx/html \
    --publish 8080:80 \
    nginx:stable

$ docker logs -f name  # follow the container logs
```

Then, open [http://localhost:8080/]() in the browser. Edit the `html/index.html`
file, and reload the page.

## Clean up

```bash
$ docker ps  # view running containers
$ docker kill name  # 'shut down' container
$ docker images  # view local images
$ docker system prune  # clean up, optional flags: `--all --force` 
```