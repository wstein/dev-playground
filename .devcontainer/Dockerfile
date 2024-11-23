ARG BASE_IMAGE=mcr.microsoft.com/dotnet/sdk:8.0-noble
FROM $BASE_IMAGE

RUN apt-get update &&\
    apt-get upgrade -y &&\
    apt-get install -y --no-install-recommends \
    bison \
    clojure \
    curl \
    file \
    flex \
    gcc \
    git \
    git-lfs \
    gitless \
    gpg \
    groovy \
    nodejs \
    htop \
    kotlin \
    libglib2.0-dev \
    libgraphviz-dev \
    maven \
    python3 \
    ruby \
    sbcl \
    swi-prolog \
    unzip \
    valac \
    xz-utils \
    zip &&\
    apt-get autoremove -y &&\
    apt-get clean -y

# Install Fast Node Manager
RUN curl -fsSL https://fnm.vercel.app/install | bash -s -- --install-dir "/usr/local/bin"

# Install Crystal Lang
RUN curl -fsSL https://packagecloud.io/84codes/crystal/gpgkey | gpg --dearmor | tee /etc/apt/trusted.gpg.d/84codes_crystal.gpg > /dev/null &&\
    echo "deb https://packagecloud.io/84codes/crystal/any any main" | tee /etc/apt/sources.list.d/84codes_crystal.list &&\
    apt-get update &&\
    apt-get install -y --no-install-recommends crystal fzf fd-find neovim python3-pip &&\
    apt-get remove -y --purge vim &&\
    ln -s /usr/bin/nvim /usr/local/bin/vim &&\
    ln -s /usr/bin/fdfind /usr/bin/fd &&\
    curl -o /usr/local/lib/antlr-4-complete.jar https://www.antlr.org/download/antlr-4.13.2-complete.jar &&\
    apt-get autoremove -y &&\
    apt-get clean -y

ENV PATH=$PATH:/home/vscode/.dotnet:/home/vscode/.dotnet/tools
ENV CLASSPATH=".:/usr/local/lib/antlr-4-complete.jar"

RUN if [ ! -f /usr/share/doc/fzf/examples/key-bindings.zsh ]; then \
    wget -O /usr/share/doc/fzf/examples/key-bindings.zsh https://raw.githubusercontent.com/junegunn/fzf/refs/heads/master/shell/key-bindings.zsh; \
    fi;

RUN if id "ubuntu" &>/dev/null; then \
    echo "Deleting user 'ubuntu'" && userdel -f -r ubuntu || echo "Failed to delete ubuntu user"; \
    fi;

# clear this environment variable so xml docs from NuGet packages are unpackaged. The default dotnet/sdk image sets it to 'skip'.
# see https://github.com/dotnet/dotnet-docker/issues/2790
ENV NUGET_XMLDOC_MODE=
