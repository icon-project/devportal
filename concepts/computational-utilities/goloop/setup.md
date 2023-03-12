# Setup

### Prerequisites

Install OpenJDK version 11. Visit [OpenJDK.net](http://openjdk.java.net) for prebuilt binaries. Or you can install a proper OpenJDK package from your OS vendors.

* In macOS:

    ```
    $ brew tap AdoptOpenJDK/openjdk
    $ brew install --cask adoptopenjdk11
    ```

* In Linux (Ubuntu 20.04):

    ```
    $ sudo apt install openjdk-11-jdk
    ```
    
[Download and install Go](https://go.dev/doc/install), then verify the installation.

```
$ go version
```

Clone the `goloop` repository in a folder in your machine.

```
$ git clone https://github.com/icon-project/goloop.git
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
goloop version v1.3.3-34-gbed5066e linux/amd64 tags()-2023-02-24-23:35:14
```

Lastly we create an alias for `goloop`. Open your `~/.profile` file and add the following at the end.

```
alias goloop=”path/to/goloop/bin/goloop”
```

After adding the alias either open a new terminal or source the `~/.profile` file
```
source ~/.profile
```
