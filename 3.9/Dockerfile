FROM ubuntu:22.04 as builder
ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Etc/UTC

RUN apt-get update && apt-get install -y wget
RUN apt-get install -y --no-install-recommends build-essential pkg-config \
      libbz2-dev libffi-dev libreadline6-dev libssl-dev uuid-dev zlib1g-dev
RUN apt-get install -y --no-install-recommends pipx && pipx install virtualenv

RUN wget https://www.python.org/ftp/python/3.9.13/Python-3.9.13.tgz \
      && tar -xvf Python-3.9.13.tgz
WORKDIR /Python-3.9.13/
# Optimization level three (the default) can cause the build to take forever
# on my potato laptop.
ENV CFLAGS="-O0 -g0"
RUN ./configure --with-pydebug
RUN make -j
RUN /root/.local/bin/virtualenv venv --python=/Python-3.9.13/python

FROM ubuntu:22.04
COPY --from=builder /Python-3.9.13/ /Python-3.9.13/
CMD ["/Python-3.9.13/venv/bin/python"]
