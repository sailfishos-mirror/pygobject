FROM ubuntu:noble

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    software-properties-common \
    build-essential \
    ccache \
    curl \
    dbus \
    gir1.2-freedesktop-dev \
    gir1.2-girepository-3.0-dev \
    gir1.2-gtk-3.0 \
    gir1.2-gtk-4.0 \
    git \
    gobject-introspection \
    lcov \
    libbz2-dev \
    libcairo2-dev \
    libffi-dev \
    libgirepository-2.0-dev \
    libglib2.0-dev \
    libgtk-3-0 \
    libgtk-4-1 \
    libreadline-dev \
    libsqlite3-dev \
    libssl-dev \
    liblzma-dev \
    ninja-build \
    python3-pip \
    sudo \
    xauth \
    xvfb \
    && rm -rf /var/lib/apt/lists/*

RUN add-apt-repository -y ppa:pypy/ppa \
    && apt-get update \
    && apt-get install -y pypy3 pypy3-dev pypy3-venv \
    && rm -rf /var/lib/apt/lists/* \
    && sed -i 's#/build/pypy3-[^/]*/[^/]*#/usr#g' /usr/lib/pypy3.*/_sysconfigdata__x86_64-linux-gnu.py

ARG HOST_USER_ID=5555
ENV HOST_USER_ID=${HOST_USER_ID}
RUN useradd -u $HOST_USER_ID -ms /bin/bash user \
    && echo 'user ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

USER user
WORKDIR /home/user

ENV LANG=C.UTF-8
ENV CI=true
ENV PYENV_ROOT=/home/user/.pyenv
ENV PATH="${PYENV_ROOT}/shims:${PYENV_ROOT}/bin:${PATH}"
ENV PYTHON_CONFIGURE_OPTS="--enable-shared"

# https://github.com/pyenv/pyenv/releases
ENV PYENV_GIT_TAG=v2.4.17
RUN curl -L https://raw.githubusercontent.com/pyenv/pyenv-installer/master/bin/pyenv-installer | bash

# Register the pypy3 system installation
RUN mkdir -p "${PYENV_ROOT}/versions/pypy3-system/bin/" \
    && sudo ln -s /usr/bin/pypy3 "${PYENV_ROOT}/versions/pypy3-system/bin/python" \
    && pyenv rehash

RUN pyenv install --debug 3.9
RUN pyenv install --debug 3.10
RUN pyenv install --debug 3.11
RUN pyenv install --debug 3.12
RUN pyenv install --debug 3.13

ENV PATH="/usr/lib/ccache:${PATH}"

COPY --chown=user:user set_env.sh /home/user/set_env.sh
RUN chmod +x /home/user/set_env.sh
ENTRYPOINT ["/home/user/set_env.sh"]