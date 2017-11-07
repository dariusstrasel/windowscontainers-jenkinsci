# windowscontainers-jenkinsci
Dockerfiles for - Jenkins CI on Windows Containers for Windows Server Core

The blog post: http://blog.alexellis.io/continuous-integration-docker-windows-containers/

## Installation/Setup Instructions
### Build this as a local image:

```
docker build -t windows-java:jre1.8.0_91 .
```

### Build the image:

```
docker build -t jenkins-windows:2.0 .
```

### You can now run your Jenkins master:

```
$ docker run --name jenkinsci -p 8080:8080 -p 50000:50000 -d jenkins-windows:2.0
```

### Since the Jenkins 2.0 release security is enabled by default and you will need to find the initial password from the Jenkins container before you can login.

```
$ docker logs jenkinsci
```

### Connecting to Windows Containers:
Type in docker inspect jenkinsci
Look for the (NAT) IP address of the container
For Windows 10 - use that IP address in place of localhost. For Windows 2016 use the NAT IP address or the IP address of one of your Ethernet adapters such as http://10.95.11.1:8080.

### Create a Jenkins agent image
For the build pass in the BASE_URL environmental variable like this:
```
$ docker build --build-arg BASE_URL=http://192.168.0.101:8080 -t jenkins_windows_agent:2.0 .
```

### Run your agent:
```
$ docker run -ti jenkins_windows_agent -jnlpUrl http://192.168.0.101:8080/computer/Windows/slave-agent.jnlp -secret e9714c100fb003e2cef3609b96a255da5f488bc5f195ef6a0fafcebb2836d4e3
```


