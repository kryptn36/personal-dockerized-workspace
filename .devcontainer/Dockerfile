FROM ubuntu:18.04

RUN apt update && apt install -y curl git unzip xz-utils zip libglu1-mesa openjdk-8-jdk wget nano

# Set up new user
RUN useradd -ms /bin/bash developer
USER developer
WORKDIR /home/developer
RUN mkdir -p Extra

# Install GraalVM
RUN curl -O -L https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-20.1.0/graalvm-ce-java11-linux-amd64-20.1.0.tar.gz
RUN tar xvfz graalvm-ce-java11-linux-amd64-20.1.0.tar.gz && rm graalvm-ce-java11-linux-amd64-20.1.0.tar.gz
RUN mv graalvm-ce-java11-20.1.0/ Extra/GraalVM
ENV PATH "/home/developer/Extra/GraalVM/bin:$PATH"

# Install maven
RUN curl -O -L https://downloads.apache.org/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
RUN tar xvfz apache-maven-3.6.3-bin.tar.gz && rm apache-maven-3.6.3-bin.tar.gz
RUN mv apache-maven-3.6.3/ Extra/Maven
ENV PATH "/home/developer/Extra/Maven/bin:$PATH"

# Install node
RUN curl -O -L https://nodejs.org/dist/v12.18.2/node-v12.18.2-linux-x64.tar.xz
RUN tar xvf node-v12.18.2-linux-x64.tar.xz && rm node-v12.18.2-linux-x64.tar.xz
RUN mv node-v12.18.2-linux-x64/ Extra/Node
ENV PATH "/home/developer/Extra/Node/bin:$PATH"

# Prepare Android directories and system variables
RUN mkdir -p Android/sdk
ENV ANDROID_SDK_ROOT /home/developer/Android/sdk
RUN mkdir -p .android && touch .android/repositories.cfg

# Set up Android SDK
RUN wget -O sdk-tools.zip https://dl.google.com/android/repository/sdk-tools-linux-4333796.zip
RUN unzip sdk-tools.zip && rm sdk-tools.zip
RUN mv tools Android/sdk/tools
RUN cd Android/sdk/tools/bin && sed -i '1iJAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64' sdkmanager
RUN cd Android/sdk/tools/bin && sed -i '1iPATH=/usr/lib/jvm/java-8-openjdk-amd64/bin:$PATH' sdkmanager
RUN cd Android/sdk/tools/bin && yes | ./sdkmanager --licenses
RUN cd Android/sdk/tools/bin && ./sdkmanager "build-tools;29.0.2" "patcher;v4" "platform-tools" "platforms;android-29" "sources;android-29"
ENV PATH "$PATH:/home/developer/Android/sdk/platform-tools"

# Download Flutter SDK
RUN git clone https://github.com/flutter/flutter.git
ENV PATH "$PATH:/home/developer/flutter/bin"

# Run basic check to download Dark SDK
RUN flutter doctor