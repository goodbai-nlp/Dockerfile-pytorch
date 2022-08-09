ARG UBUNTU_VERSION=20.04

FROM ubuntu:${UBUNTU_VERSION} as base

RUN apt-get update && apt install -y apt-transport-https && \
    apt-get install -y openjdk-8-jdk wget curl vim python python3 gnupg2 && \
    echo "deb https://repo.scala-sbt.org/scalasbt/debian all main" | tee /etc/apt/sources.list.d/sbt.list && \
    echo "deb https://repo.scala-sbt.org/scalasbt/debian /" | tee /etc/apt/sources.list.d/sbt_old.list && \
    curl -sL "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x2EE0EA64E40A89B84B2DF73499E82A75642AC823" | apt-key add && \
    apt-get update && \
    apt-get install -y sbt=1.3.13 && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
