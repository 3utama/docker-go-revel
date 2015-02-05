FROM buildpack-deps:jessie-scm

# gcc for cgo
RUN apt-get update && apt-get install -y \
		gcc libc6-dev make \
		--no-install-recommends \
	&& rm -rf /var/lib/apt/lists/*

ENV GOLANG_VERSION 1.4.1

RUN curl -sSL https://golang.org/dl/go$GOLANG_VERSION.src.tar.gz \
		| tar -v -C /usr/src -xz

RUN cd /usr/src/go/src && ./make.bash --no-clean 2>&1

ENV PATH /usr/src/go/bin:$PATH

RUN mkdir -p /go/src
ENV GOPATH /go
ENV PATH /go/bin:$PATH
WORKDIR /go

COPY go-wrapper /usr/local/bin/

# revel
RUN go get github.com/revel/revel
RUN go get github.com/revel/cmd/revel
ENV PATH $PATH:$GOPATH/bin
RUN echo 'export PATH="$PATH:$GOPATH/bin"' >> ~/.bash_profile
WORKDIR $GOPATH/src
RUN git clone http://github.com/3utama/practice-revel.git
WORKDIR $GOPATH
CMD ["/bin/bash", "-c", "revel run practice-revel"]
