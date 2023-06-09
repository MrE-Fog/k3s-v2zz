# Generate Build Container

1. set env variable PATH_TO_KUBERNETES_REPO to the path to your local kubernetes/kubernetes copy: `export PATH_TO_KUBERNETES_REPO="/Users/mtrachier/go/src/github.com/kubernetes/kubernetes"`
1. set env variable GOVERSION to the expected version of go for the kubernetes/kubernetes version checked out: `export GOVERSION=$(yq -e '.dependencies[] | select(.name == "golang: upstream version").version' $PATH_TO_KUBERNETES_REPO/build/dependencies.yaml)`
1. set env variable GOIMAGE to the expected container image to base our custom build image on: `export GOIMAGE="golang:${GOVERSION}-alpine3.15"`
1. set env variable BUILD_CONTAINER to the contents of a dockerfile for the build container: `export BUILD_CONTAINER="FROM ${GOIMAGE}\nRUN apk add --no-cache bash git make tar gzip curl git coreutils rsync alpine-sdk"`
1. use Docker to create the build container: `echo -e $BUILD_CONTAINER | docker build -t ${GOIMAGE}-dev -`
