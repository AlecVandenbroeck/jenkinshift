## Building

 * install [go version 1.5.1 or later](https://golang.org/doc/install)
 * install [glide](https://github.com/Masterminds/glide#install)
 * type the following:

```
cd $GOPATH
mkdir -p src/github.com/fabric8io/
cd src/github.com/fabric8io/
git clone https://github.com/fabric8io/jenkinshift.git
cd fabric8io

make bootstrap
```

 * then to build the binary

     make build

 * you can then run it via

     ./bin/jenkinshift

### Running pods locally

You can run `jenkinshift rc ...` easily on a local build when working on the code. However to try out changes to the pod for `jenkinshift pod ...` you can run that locally outside of docker with a small trick.

You must set the `HOSTNAME` environment variable to a valid pod name you wish to use.

```bash
    export HOSTNAME=fabric8-znuj5
```

e.g. the above uses the pod name for the current fabric8 console.

This lets you pretend to be different pods from the command line when trying it out locally. e.g. run the `jenkinshift pod ...` command in 2 shells as different pods, provided the `HOSTNAME` values are diferent.

The pod name must be valid as `jenkinshift pod ...` command will update the pod to annotate which host its chosen etc.

So to run the [above examples](#running-jenkinshift) type the following:

for [fabric8-ansible-spring-boot](https://github.com/fabric8io/fabric8-ansible-spring-boot):

    jenkinshift pod -rc hawtapp-demo appservers /opt/cdi-camel-2.2.98-SNAPSHOT-app/bin/run.sh

for [fabric8-ansible-hawtapp](https://github.com/fabric8io/fabric8-ansible-hawtapp):

    jenkinshift pod  -rc springboot-demo appservers /opt/springboot-camel-2.2.98-SNAPSHOT

### Working with Windows

If you're like me and have used a Mac for years, you might have forgotten how to work with Windows boxes ;). Here's some tips on how to test things out on the Windows VMs

First install the [winrm binary](http://github.com/masterzen/winrm/) which you can do if you have golang installed via:

    go get github.com/masterzen/winrm

Then to connect to one of the Windows VMs from an example, such as the [fabric8-ansible-hawtapp](https://github.com/fabric8io/fabric8-ansible-hawtapp) you can use:

    winrm -hostname 10.10.3.21 -username IEUser -password 'Passw0rd!' 'cmd'

Then you can test if a java process is running via

    jps -l

If you wish to kill a java process from its PID you can type:

    taskkill /PID 4308 /F

Enjoy!    

## Releasing

Just run `make release`. This will cross-compile for all supported platforms, create tag & upload tarballs (zip file for Windows) to Github releases for download.

Updating the version is done via `make bump` to bump minor version & `make bump-patch` to bump patch version. This is necessary as tags are created from the version specified when releasing.
