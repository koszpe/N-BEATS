FROM pytorch/pytorch

# Install some basic apt packages
RUN DEBIAN_FRONTEND=noninteractive apt-get update -qq
RUN    DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -qy \
    sudo vim nano git curl wget mc less \
    openssh-server \
    ffmpeg libsm6 libxext6 enchant

RUN echo "%docker  ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers.d/inside_sudoers

RUN pip install tqdm dotmap matplotlib opencv-python pandas numpy jupyter sklearn tensorboard pudb pyenchant openpyxl tabulate shap captum

################################
# install pyodb and dependencies
RUN DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -qy  \
    g++ gcc lsb-core unixodbc-dev gnupg

RUN pip install pyodbc

RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
RUN curl https://packages.microsoft.com/config/ubuntu/$(lsb_release -rs)/prod.list > /etc/apt/sources.list.d/mssql-release.list

RUN DEBIAN_FRONTEND=noninteractive apt-get update -qq
RUN DEBIAN_FRONTEND=noninteractive ACCEPT_EULA=Y apt-get install -y \
    mssql-tools18 msodbcsql18

ENV PATH="$PATH:/opt/mssql-tools18/bin"
################################


ENV PYTHONPATH=/workspace
WORKDIR /workspace

# clean apt meta
RUN apt-get clean -qq && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /tmp/*

COPY entry.sh /entry.sh
RUN chmod +x /entry.sh
ENTRYPOINT ["/entry.sh"]
