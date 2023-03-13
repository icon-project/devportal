# Setup

### Prerequisites

*   You need to install OpenJDK 11 version. Visit [OpenJDK.net](http://openjdk.java.net) for prebuilt binaries. Or you can install a proper OpenJDK package from your OS vendors.

    In macOS:

    ```
    $ brew tap AdoptOpenJDK/openjdk
    $ brew install --cask adoptopenjdk11
    ```

    In Linux (Ubuntu 20.04):

    ```
    $ sudo apt install openjdk-11-jdk
    ```
*   Download and Install Go

    In macOS:

    ```
    $ brew install go
    ```

    In Linux (Ubuntu 20.04):

    ```
    $ wget -c https://go.dev/dl/go1.16.15.linux-amd64.tar.gz -O - | sudo tar -xz -C /usr/local
    $ export PATH=$PATH:/usr/local/go/bin
    ```

    Verify installation

    ```
    $ go version
    ```

## Step 1. Build goloop CLI

You need to checkout the "goloop" repository for building the `goloop` CLI.

```
$ git clone git@github.com:icon-project/goloop.git
$ GOLOOP_ROOT=/path/to/goloop
```

Then build the `goloop` CLI binary.

```
$ cd ${GOLOOP_ROOT}
$ GOBUILD_TAGS= make goloop
```

Make sure the binary builds successfully.

```
$ ./bin/goloop version
goloop version v1.2.3-7-g78f7f438 linux/amd64 tags()-2022-03-21-14:09:46
```
