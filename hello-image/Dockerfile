FROM centos:centos7.9.2009 AS builder
EXPOSE 8080/tcp
WORKDIR /app
COPY . /app
RUN yum update -y && \
    yum install epel-release wget unzip git -y && \
    wget https://download.java.net/java/GA/jdk16.0.1/7147401fd7354114ac51ef3e1328291f/9/GPL/openjdk-16.0.1_linux-x64_bin.tar.gz && \
    wget https://services.gradle.org/distributions/gradle-7.4.2-all.zip && \
    wget https://repo.spring.io/artifactory/release/org/springframework/boot/spring-boot-cli/2.6.6/spring-boot-cli-2.6.6-bin.zip && \
    tar -xvf openjdk-16.0.1_linux-x64_bin.tar.gz && \
    unzip gradle-7.4.2-all.zip && \
    unzip spring-boot-cli-2.6.6-bin.zip
ENV JAVA_HOME=/app/jdk-16.0.1
ENV PATH=$PATH:$JAVA_HOME/bin/
ENV GRADLE_HOME=/app/gradle-7.4.2
ENV PATH=$PATH:$GRADLE_HOME/bin/
ENV SPRING_HOME=/app/spring-2.6.6
ENV PATH=$PATH:$SPRING_HOME/bin/
RUN git clone https://github.com/pdcong/HelloWorld.git && \
    spring init --build=gradle --dependencies=web --name=hello hello-world && \
    cp HelloWorld/HelloApp.java /app/hello-world/src/main/java/com/example/helloworld/HelloApplication.java && \
    cd /app/hello-world/ && ./gradlew build


FROM softfocus/openjdk:latest
WORKDIR /runapp
COPY --from=builder /app/hello-world/build/libs/hello-world-0.0.1-SNAPSHOT.jar /runapp/
ENTRYPOINT ["java","-jar","/runapp/hello-world-0.0.1-SNAPSHOT.jar"]