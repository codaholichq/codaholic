---
slug: java-docker-best-practices
title: "Java Docker Best Practices"
date: 2023-08-20T12:38:53+01:00
description: "Discover essential Java Docker best practices. Optimize containerized Java applications for performance, scalability, and seamless deployment. 🚀 #JavaDocker"
draft: false
cover:
    image: img/00-java-docker-best-practices.webp
    alt: 'Java Docker Best Practices'
    caption: 'Java Docker Best Practices'
---

We all want to avoid the `it was working perfectly well on my system` scenario. Hence, there is a need to dockerise our Java web application. This tutorial gives us the best practices to consider when containerising our Java applications.

Below are the best practices to keep in mind when containerising or dockerising Java web applications:
1. Specify Explicit and Predictable Docker Base Image Tags
2. Install Only What You Need in Production in the Java Container Image
3. Find and Fix Security Vulnerabilities in Your Java Docker Image
4. Use Multi-Stage Builds
5. Don’t Run Java Apps as Root
6. Properly Handle Events to Safely Terminate a Java Application
7. Gracefully Tear Down Java Applications
8. Use .dockerignore
9. Make Sure Java is Container-Aware
10. Be Careful with Automatic Docker Container Generation Tools

Let's look at these best practices and properly understand how to apply them.

1.  **Specify Explicit and Predictable Docker Base Image Tags**: Choose specific and versioned base images for your Java containers. Avoid using generic tags like "latest," as it may lead to unexpected changes in your environment over time. Explicitly specifying tags ensures consistency and predictability in your container environment.

#### Maven
```docker
FROM maven:3.9.5-eclipse-temurin-21-alpine@sha256:7b4c4ff7e066658ade018542494add4db8b871aebb0aeedd0cf016f75c36084b
RUN mkdir /app
WORKDIR /app
COPY . /app
RUN mvn package -Dmaven.test.skip=true
```

#### Gradle
```docker
FROM gradle:8.3@sha256:68ce1cd457891f48d1e137c7d6a4493f60843e84c9e2634e3df1d3d5b381d36c
RUN mkdir /app
WORKDIR /app
COPY . /app
RUN gradle clean build -x test
```

<br/>

#### How To Get An Explicit and Predictable Docker Base Image Tag
Open command prompt or powershell if you are on a PC or open the terminal if you are on a Mac and enter the below
```shell
docker manifest inspect amazoncorretto:21-alpine
```

`amazoncorretto` is the image
`21-alpine` is the tag

<br/>

2. **Install Only What You Need in Production in the Java Container Image**: Keep your container images lean by including only the necessary dependencies for your production application. Avoid unnecessary packages and libraries to reduce image size, improve security, and minimize potential vulnerabilities.
#### Maven
```docker
FROM maven:3.9.5-eclipse-temurin-21-alpine@sha256:7b4c4ff7e066658ade018542494add4db8b871aebb0aeedd0cf016f75c36084b
RUN mkdir /app
COPY ./target/*.jar /app/app.jar
WORKDIR /app
EXPOSE 80
CMD "java" "-jar" "app.jar"
```

#### Gradle
```
FROM gradle:8.3@sha256:68ce1cd457891f48d1e137c7d6a4493f60843e84c9e2634e3df1d3d5b381d36c
RUN mkdir /app
COPY ./build/libs/*.jar /app/app.jar
WORKDIR /app
EXPOSE 80
CMD ["java", "-jar", "app.jar"]
```
<br/>

3. **Find and Fix Security Vulnerabilities in Your Java Docker Image**: Regularly scan your container images for security vulnerabilities using tools like vulnerability scanners. Address identified vulnerabilities promptly by updating packages, libraries, or configurations to maintain a secure environment. For this guide, we are going to use `Docker Bench`. Docker Bench is a script with multiple automated tests that checks for the best practices for deploying containers on production.

To use this tool, you must install Docker 1.13.0 or later.

<br/>

4. **Use Multi-Stage Builds**: Utilize multi-stage Docker builds to separate build-time dependencies from the final runtime image. This approach helps create smaller final images by discarding unnecessary build-time artifacts and focusing on runtime essentials.
#### Maven
```docker
FROM maven:3.9.5-eclipse-temurin-21-alpine@sha256:7b4c4ff7e066658ade018542494add4db8b871aebb0aeedd0cf016f75c36084b AS build
RUN mkdir /project
COPY . /project
WORKDIR /project
RUN mvn package -Dmaven.test.skip=true

FROM amazoncorretto:21-alpine@sha256:696d03bfaeae332077e0ed579845c692949567944837dd99dd2f4fce405c806e
RUN mkdir /app
COPY --from=build /project/target/*.jar /app/app.jar
WORKDIR /app
EXPOSE 80
CMD "java" "-jar" "app.jar"
```

#### Gradle
```docker
FROM gradle:8.3@sha256:68ce1cd457891f48d1e137c7d6a4493f60843e84c9e2634e3df1d3d5b381d36c AS build
RUN mkdir /project
COPY . /project
WORKDIR /project
RUN gradle clean build -x test

FROM amazoncorretto:21-alpine@sha256:696d03bfaeae332077e0ed579845c692949567944837dd99dd2f4fce405c806e
RUN mkdir /app
COPY --from=build /project/build/libs/*.jar /app/app.jar
WORKDIR /app
EXPOSE 80
CMD "java" "-jar" "app.jar"
```
<br/>

5. **Don’t Run Java Apps as Root**: In software, we have a principle called the Principle of Least Privilege, which is also known as the Principle of Least Authority. This is an important principle in computer security, as it promotes minimal privileges on computers based on a user’s job necessities. This helps to reduce the "attack surface" of the computer by removing unneeded privileges that can result in computer compromises or network exploits. You can apply this concept to the computers you work on by just operating without administrative rights.

Applications are usually designed to run with limited security permissions, so when they need to do something that requires high-level access, they have to "elevate" their privileges. This is a standard security practice.

Running your application without the highest-level access (like "root" on Unix-based systems) makes it easier to install and scale. In simple terms, the less special access your application needs, the smoother it can fit into a larger computing environment.

#### Maven
```docker
FROM maven:3.9.5-eclipse-temurin-21-alpine@sha256:7b4c4ff7e066658ade018542494add4db8b871aebb0aeedd0cf016f75c36084b AS build
RUN mkdir /project
COPY . /project
WORKDIR /project
RUN mvn package -Dmaven.test.skip=true
 
 
FROM amazoncorretto:21-alpine@sha256:696d03bfaeae332077e0ed579845c692949567944837dd99dd2f4fce405c806e
RUN mkdir /app
RUN addgroup --system myuser && adduser -S -s /bin/false -G myuser myuser
COPY --from=build /project/target/*.jar /app/app.jar
WORKDIR /app
RUN chown -R myuser:myuser /app
USER myuser
EXPOSE 80
CMD "java" "-jar" "app.jar"
```

#### Gradle
```docker
FROM gradle:8.3@sha256:68ce1cd457891f48d1e137c7d6a4493f60843e84c9e2634e3df1d3d5b381d36c AS build
RUN mkdir /project
COPY . /project
WORKDIR /project
RUN gradle clean build -x test
 
 
FROM amazoncorretto:21-alpine@sha256:696d03bfaeae332077e0ed579845c692949567944837dd99dd2f4fce405c806e
RUN mkdir /app
RUN addgroup --system myuser && adduser -S -s /bin/false -G myuser myuser
COPY --from=build /project/target/*.jar /app/app.jar
WORKDIR /app
RUN chown -R myuser:myuser /app
USER myuser
EXPOSE 80
CMD "java" "-jar" "app.jar"
```
<br/>

6. **Properly Handle Events to Safely Terminate a Java Application**: Implement proper shutdown handling in your Java application to gracefully handle termination signals from Docker. This ensures that your application releases resources and performs necessary cleanup before shutting down.

#### Maven
```docker
FROM maven:3.9.5-eclipse-temurin-21-alpine@sha256:7b4c4ff7e066658ade018542494add4db8b871aebb0aeedd0cf016f75c36084b AS build
RUN mkdir /project
COPY . /project
WORKDIR /project
RUN mvn package -Dmaven.test.skip=true
 
FROM amazoncorretto:21-alpine@sha256:696d03bfaeae332077e0ed579845c692949567944837dd99dd2f4fce405c806e
RUN apk add dumb-init
RUN mkdir /app
RUN addgroup --system myuser && adduser -S -s /bin/false -G myuser myuser
COPY --from=build /project/target/*.jar /app/app.jar
WORKDIR /app
RUN chown -R myuser:myuser /app
USER myuser
EXPOSE 80
CMD "dumb-init" "java" "-jar" "app.jar"
```

#### Gradle
```docker
# Build stage
FROM gradle:8.3@sha256:68ce1cd457891f48d1e137c7d6a4493f60843e84c9e2634e3df1d3d5b381d36c AS build
LABEL maintainer="codaholic.com"
WORKDIR /
COPY . /
RUN gradle build --no-daemon -x test

# Package stage
FROM amazoncorretto:21-alpine@sha256:696d03bfaeae332077e0ed579845c692949567944837dd99dd2f4fce405c806e
RUN apk add --no-cache dumb-init
RUN mkdir /app
RUN addgroup --system myuser && adduser -S -s /bin/false -G myuser myuser
COPY --from=build /target/libs/*.jar /app/app.jar
WORKDIR /app
RUN chown -R myuser:myuser /app
USER myuser
EXPOSE 80
ENTRYPOINT ["dumb-init", "java", "-jar", "app.jar"]
```
<br/>

7. **Gracefully Tear Down Java Applications**: Consider using frameworks or techniques that allow your Java application to gracefully handle unexpected errors or crashes. Graceful shutdowns minimize potential data loss or corruption during container restarts.
```java
Runtime.getRuntime().addShutdownHook(new Thread() {
   @Override
   public void run() {
       System.out.println("Inside Add Shutdown Hook");
   }
});
```
<br/>

8. **Use .dockerignore**: Create a `.dockerignore` file to specify files and directories that should be excluded from the Docker build context. This helps reduce build times and prevents unnecessary files from being included in the image.

```docker
# Keeping unneeded files out of your Java container images
.logs
target

.mvn
.gradle
gradle

.idea
.vscode

.git
.gitignore

Dockerfile
.dockerignore
```
<br/>

9. **Make Sure Java is Container-Aware**: Ensure your Java application is aware of its containerized environment. Adjust resource limits, configurations, and logging mechanisms to match the container environment, improving compatibility and performance.
<br/>

10. **Be Careful with Automatic Docker Container Generation Tools**: While tools that automate container creation can be helpful, carefully review and understand their generated Dockerfiles. They may not adhere to best practices or align with your application's requirements. Modify generated files as needed to meet your standards.

Implementing these best practices when building Java containers will lead to more efficient, secure, and reliable containerized Java applications.

<br/>

{{< bmc-button slug="codaholic" >}}

<br/>

### Frequently Asked Questions

**Is Docker useful for Java?**
Docker enables you to establish uniform environments for development, testing, and production, ensuring your Java application packaged in a Docker container will function identically across all environments, minimizing the likelihood of unexpected problems during deployment.

**Does each Docker container have its own JVM?**
In essence, each container would operate its own separate Java process, thereby necessitating the use of its own JVM. However, you may find it beneficial to organize your applications in a way that instead of having numerous apps running on a single server or VM, you have multiple containers each hosting several apps on a single server.

**What is the recommended heap size for Java?**
It is generally advisable to limit the Java heap size to no more than half of the total available RAM on the server. Increasing the heap size beyond this threshold can lead to performance issues. For instance, if a server has 16 GB of RAM, the maximum heap size should not exceed 8 GB to avoid any adverse effects on performance.