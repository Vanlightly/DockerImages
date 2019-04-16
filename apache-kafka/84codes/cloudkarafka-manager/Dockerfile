FROM ubuntu:16.04

RUN apt-get update
RUN apt-get install -y libsasl2-2 libssl1.0.0
RUN apt-get install -y net-tools
RUN apt-get install -y curl
RUN apt-get install -y git
RUN apt-get install -y wget
RUN apt-get install -y software-properties-common
RUN apt-get install -y apt-transport-https
RUN apt-get install -y pkg-config

RUN mkdir /tmp-files
WORKDIR /tmp-files

# install librdkafka
RUN wget -qO - https://packages.confluent.io/deb/5.2/archive.key | apt-key add -
RUN add-apt-repository "deb [arch=amd64] https://packages.confluent.io/deb/5.2 stable main"
RUN apt-get update
RUN apt-get install -y librdkafka-dev librdkafka1

# install Go
RUN wget https://dl.google.com/go/go1.11.linux-amd64.tar.gz
RUN tar -xvf go1.11.linux-amd64.tar.gz
RUN mv go /usr/local
RUN mkdir -p /go/bin

ENV GOROOT=/usr/local/go
ENV GOPATH=/go
ENV PATH=${GOPATH}/bin:${GOROOT}/bin:${PATH}

# get the code
RUN mkdir -p /go/src/github.com/CloudKarafka
WORKDIR /go/src/github.com/CloudKarafka
RUN git clone https://github.com/CloudKarafka/cloudkarafka-manager.git

# install dep into sourcxe directory
WORKDIR /go/bin
RUN curl https://raw.githubusercontent.com/golang/dep/master/install.sh | sh
RUN cp dep /go/src/github.com/CloudKarafka/cloudkarafka-manager
RUN rm -rf /tmp-files

# run dep and run the go app
WORKDIR /go/src/github.com/CloudKarafka/cloudkarafka-manager
RUN dep ensure -update
CMD go run app.go --authentication=$AUTH_MODE --zookeeper=$ZOOKEEPER