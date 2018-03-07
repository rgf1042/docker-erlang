# Set the base image
FROM debian:stretch

LABEL version="1.0"
LABEL description="This docker image is ready for \
developing with erlang software."
LABEL maintainer="roger.garcia@guifi.net"

ENV NODE_ROOT_DIR /usr/share/node

RUN apt-get update && apt-get dist-upgrade -y \
  && apt-get install -y libwxgtk3.0-dev libgl1-mesa-dev \
  libpng-dev libglu1-mesa build-essential \
  curl libncurses5-dev \
  && apt-get clean \
  && apt-get autoremove \
  && rm -rf /var/lib/apt/lists/*

# Download erlang otp 20.2
WORKDIR /tmp
RUN curl http://erlang.org/download/otp_src_20.2.tar.gz | \
  tar -zxv


# Compile and install
WORKDIR /tmp/otp_src_20.2
RUN ./configure && make && make install

# Remove useless files
RUN rm -rf /tmp/otp_src_20.2

# Remove useless packages
RUN apt-get remove -y libwxgtk3.0-dev libgl1-mesa-dev \
  libpng-dev libglu1-mesa build-essential \
  libncurses5-dev \
  && apt-get clean \
  && apt-get autoremove -y \
  && rm -rf /var/lib/apt/lists/*

# Creating UNIX User for development (security reasons)
RUN groupadd --system development && useradd --system --gid development development --create-home


EXPOSE 3000

CMD ["/bin/bash"]
