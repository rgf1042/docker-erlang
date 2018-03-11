# Set the base image
FROM debian:stretch

LABEL version="1.0"
LABEL description="This docker image is ready for \
developing with erlang software."
LABEL maintainer="roger.garcia@guifi.net"

ENV USER=development USER_ID=1000 USER_GID=1000

RUN apt-get update && apt-get dist-upgrade -y \
  && apt-get install -y libwxgtk3.0 libwxgtk3.0-dev \
  libgl1-mesa-glx libgl1-mesa-dev gosu \
  libpng-dev libglu1-mesa build-essential \
  curl libncurses5-dev libpng16-16 \
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
WORKDIR /
RUN rm -rf /tmp/otp_src_20.2

# Remove useless packages
RUN apt-get remove -y libwxgtk3.0-dev libgl1-mesa-dev \
  libpng-dev libglu1-mesa build-essential \
  && apt-get clean \
  && apt-get autoremove -y \
  && rm -rf /var/lib/apt/lists/*

# Creating UNIX User for development (security reasons)
RUN groupadd --gid "${USER_GID}" "${USER}" && \
    useradd \
      --uid ${USER_ID} \
      --gid ${USER_GID} \
      --create-home \
      --shell /bin/bash \
      ${USER}

# Copying script for setting userid and gid (host)
COPY user-mapping.sh /
RUN  chmod u+x user-mapping.sh

# Copying entrypoint
COPY docker-entrypoint.sh /
RUN  chmod u+x docker-entrypoint.sh

# Setting user home as working dir
WORKDIR /home/"${USER}"
# Running entrypoint script
ENTRYPOINT ["/docker-entrypoint.sh"]

CMD ["gosu","${USER}","/bin/bash"]
