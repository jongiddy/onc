Run a Docker container, as the current user, with the current directory mounted.

If the Docker image starts with `/` or `.` (after path expansion) use the image built from a Dockerfile:

```
jon:~ $ onc ./Dockerfile        # build and run Dockerfile image
jon:~ $ onc ./Dockerfile.build  # build and run Dockerfile.build image
jon:~ $ onc ~/other/Dockerfile  # build and run /home/jon/other/Dockerfile image
```

Create a separate `HOME` directory in `onc-home` that can be used between runs.

Record history in a git repo.

```sh
jon:~/demo $ onc golang
golang:dc4176589fa0:demo $ GO111MODULE="on" go get sigs.k8s.io/kind@v0.3.0
go: finding sigs.k8s.io/kind v0.3.0
go: finding github.com/modern-go/reflect2 v1.0.1
...
go: extracting github.com/golang/protobuf v0.0.0-20161109072736-4bd1920723d7
go: extracting golang.org/x/text v0.3.0
golang:dc4176589fa0:demo $ ls -l onc-home/gopath/bin/kind
-rwxr-xr-x 1 jon jon 36365885 Jun 22 06:33 onc-home/gopath/bin/kind
golang:dc4176589fa0:demo $ exit

jon:~/demo $ ls -l onc-home/gopath/bin/kind
-rwxr-xr-x 1 jon jon 36365885 Jun 22 07:33 onc-home/gopath/bin/kind

jon:~/demo $ onc ~/dev/Dockerfile.ubuntu
Sending build context to Docker daemon  83.97kB
Step 1/6 : FROM ubuntu:18.04
 ---> 4c108a37151f
Step 2/6 : RUN apt-get -y update
 ---> Using cache
 ---> c689a1156ee1
Step 3/6 : RUN apt-get -y install software-properties-common
 ---> Using cache
 ---> 36d70419e060
Step 4/6 : RUN add-apt-repository -y ppa:longsleep/golang-backports
 ---> Using cache
 ---> c4b8764ed0eb
Step 5/6 : RUN apt-get -y update
 ---> Using cache
 ---> 9914576da5b3
Step 6/6 : RUN apt-get -y install sudo docker.io golang
 ---> Using cache
 ---> 310e96d40cf3
Successfully built 310e96d40cf3
Successfully tagged home/jon/dev/dockerfile.ubuntu:latest

~/dev/Dockerfile.ubuntu:4b3d4876184f:demo $ whoami
jon
~/dev/Dockerfile.ubuntu:4b3d4876184f:demo $ onc-home/gopath/bin/kind
kind creates and manages local Kubernetes clusters using Docker container 'nodes'
...
Use "kind [command] --help" for more information about a command.
~/dev/Dockerfile.ubuntu:4b3d4876184f:demo $ exit

jon:~/demo $ onc alpine whoami
jon

jon:~/demo $ git -C onc-home log
commit 7ee84dd95b9ddf276c506bc76107a8dcb1357aef (HEAD -> master)
Date:   Sat Jun 22 07:36:24 2019 +0100

    alpine whoami

commit 5020496434918ea043ebd67a91e26f891e88046d
Date:   Sat Jun 22 07:36:15 2019 +0100

    /home/jon/dev/Dockerfile.ubuntu
    +whoami
    +onc-home/gopath/bin/kind

commit 226ecd6db001f2836c2309087afc2fcb18da1594
Date:   Sat Jun 22 07:34:06 2019 +0100

    golang
    +GO111MODULE="on" go get sigs.k8s.io/kind@v0.3.0
    +ls -l onc-home/gopath/bin/kind

commit 439fe6beef85f8500264ec165432f2004fd08ddd
Date:   Sat Jun 22 07:31:58 2019 +0100

    Setup home directory

jon:~ $
```

Additional Docker options can be added before the image name. The option processing
is basic. All words before the image name must start with `-`. Use either the compact form of options or wrap options in quotes:
```
jon:~/demo $ onc -p8000:8000 "--mount type=bind,src=$(pwd)/cache,dst=/var/cache/nginx" nginx nginx -v
nginx version: nginx/1.17.0
```
