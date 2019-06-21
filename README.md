Run a Docker container, as the current user, with the current directory mounted.

Create a separate `HOME` directory in `onc-home` that can be used between runs.

Record history in a git repo.

```sh
jon:~ $ onc golang
golang:/home/jon $ GO111MODULE="on" go get sigs.k8s.io/kind@v0.3.0
go: finding sigs.k8s.io/kind v0.3.0
go: finding github.com/inconshreveable/mousetrap v1.0.0
go: finding github.com/evanphx/json-patch v4.2.0+incompatible
go: finding github.com/ghodss/yaml v1.0.0
...
go: downloading github.com/golang/protobuf v0.0.0-20161109072736-4bd1920723d7
go: extracting github.com/gogo/protobuf v1.2.1
go: extracting github.com/golang/protobuf v0.0.0-20161109072736-4bd1920723d7
go: extracting golang.org/x/text v0.3.0
golang:/home/jon $ kind
kind creates and manages local Kubernetes clusters using Docker container 'nodes'
...
Use "kind [command] --help" for more information about a command.
golang:/home/jon $ exit

jon:~ $ bin/kind
kind creates and manages local Kubernetes clusters using Docker container 'nodes'
...
Use "kind [command] --help" for more information about a command.

jon:~ $ onc ubuntu
ubuntu:/home/jon $ ls ${HOME}
gopath
ubuntu:/home/jon $ whoami
jon
ubuntu:/home/jon $ exit

jon:~ $ onc alpine whoami
jon

jon:~ $ git -C onc-home log
commit ccde19245b035938d48e73c03aa051df35b141d5 (HEAD -> master)
Date:   Fri Jun 21 06:04:05 2019 +0100

    alpine whoami

commit 102cc93d663d8a5b4e461eefaef38a83ec630aba
Date:   Fri Jun 21 06:03:56 2019 +0100

    ubuntu
    +ls ${HOME}
    +whoami

commit 7be2ba61db43f0c4883d772001ad263ec32c26e9
Date:   Fri Jun 21 05:54:58 2019 +0100

    golang
    +GO111MODULE="on" go get sigs.k8s.io/kind@v0.3.0
    +kind

commit a0ae52ed265fbc3885ef99afa68f669898565848
Date:   Fri Jun 21 05:51:06 2019 +0100

    Setup home directory
jon:~ $
```

If the Docker image starts with `/` or `.` use the image built from a Dockerfile:

```
jon:~ $ onc ./Dockerfile        # build and run Dockerfile image
jon:~ $ onc ./Dockerfile.build  # build and run Dockerfile.build image
jon:~ $ onc ~/other/Dockerfile  # build and run /home/jon/other/Dockerfile image
```
