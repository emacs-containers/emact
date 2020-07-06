FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
      make gcc libc-dev libncurses-dev \
 && rm -rf /var/lib/apt/lists/*
WORKDIR /build
COPY checksum checksum
ADD http://www.eligis.com/emacs/emact-2.54.0.tar.gz emact.tar.gz
RUN sha256sum emact.tar.gz && sha256sum -c checksum
RUN mkdir emact && tar -C emact --strip-components 1 -xf emact.tar.gz
WORKDIR /build/emact
RUN ./configure
RUN make
RUN make install

FROM debian:buster-slim
COPY --from=build /usr/local/ /usr/local/
RUN apt-get update && apt-get -y --no-install-recommends install \
      libncurses6 \
 && rm -rf /var/lib/apt/lists/*
CMD ["emact"]
