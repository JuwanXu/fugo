[![license](http://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/udhos/fugo/blob/master/LICENSE)
[![Go Report Card - invader](https://goreportcard.com/badge/github.com/udhos/fugo/invader)](https://goreportcard.com/report/github.com/udhos/fugo/invader)

# fugo
fugo - fun with Go. gomobile OpenGL game

# Table of Contents

* [QUICK START](#quick-start)
  * [Requirements](#requirements)
  * [Building the INVADER application](#building-the-invader-application)
  * [Building the ARENA server](#building-the-arena-server)
  * [How does the INVADER application locate the ARENA server?](#how-does-the-invader-application-locate-the-arena-server)

Created by [gh-md-toc](https://github.com/ekalinin/github-markdown-toc.go)

# QUICK START

Recipe:

    go get github.com/udhos/fugo
    cd ~/go/src/github.com/udhos/fugo
    ./build.sh

## Requirements

1\. Install latest Go

There are many other ways, this is a quick recipe:

    git clone github.com/udhos/update-golang
    cd update-golang
    sudo ./update-golang.sh

2\. Install Android NDK

    Install with Android Studio:
    https://developer.android.com/studio/install.html   

Then point the env var NDK to your ndk-bundle. For example:

    echo 'export NDK=$HOME/Android/Sdk/ndk-bundle' >> ~/.profile
    . ~/.profile

3\. Install gomobile

Recipe:

    go get golang.org/x/mobile/cmd/gomobile
    gomobile init -ndk $NDK

4\. Get fugo

Recipe:

    go get github.com/udhos/fugo

## Building the INVADER application

5\. Build for desktop

Recipe:

    go install github.com/udhos/fugo/demo/invader

Hint: You can test the desktop version by running 'invader':

    $ (cd demo/invader && invader slow)

The parameter 'slow' sets a very low frame rate, useful for test/debugging.
If you want smooth rendering, remove the parameter 'slow'.

The subshell is used to temporarily enter the demo/invader dir in order to load assets from demo/invader/assets).

6\. Build for Android

Recipe:

    gomobile build -target=android github.com/udhos/fugo/demo/invader

Hint: Use 'gomobile build -x' to see what the build is doing.

    $ gomobile build -x github.com/udhos/fugo/demo/invader

7\. Push into Android device

Recipe:

    gomobile install github.com/udhos/fugo/demo/invader

## Building the ARENA server

8\. Build the server

Recipe:

    $ go install github.com/udhos/fugo/arena

9\. Run the server

    $ arena

## How does the INVADER application locate the ARENA server?

The Invader application will continously try two methods to reach the server:

a) The Invader application will send a discovery request to UDP 239.1.1.1:8888. If there is an Arena server in the LAN, it will respond reporting its TCP endpoint. This local discovery is useful for quickly deploying a local Arena server. It depends on multicasting on the local network.

b) The Invader application will try to connect to the Arena server specified in the file server.txt:

    $ more demo/invader/assets/server.txt 
    localhost:8080

The TCP endpoint hard-coded in the file server.txt is included in the APK file. You will need to rebuild and redeploy the application to change it. This option is useful for deploying public Arena server on the Internet.

--xx--

