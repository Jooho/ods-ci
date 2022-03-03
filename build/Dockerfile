FROM quay.io/centos/centos:stream8

# Use this build arg to set any default test script arguments
ARG RUN_SCRIPT_ARGS=
ENV RUN_SCRIPT_ARGS=${RUN_SCRIPT_ARGS}

ENV HOME /tmp

#RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-Linux-* &&\
#    sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-Linux-*

RUN dnf -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm &&\
    dnf install -y git python39 python39-pip unzip chromium chromedriver &&\
    dnf clean all

RUN mkdir $HOME/ods-ci
# Change the WORKDIR so the run script references any files/folders from the root of the repo
WORKDIR $HOME/ods-ci

COPY tests tests/
COPY requirements.txt run_robot_test.sh setup.py .
RUN python3 -m pip install --upgrade pip
RUN python3 -m venv venv && source venv/bin/activate && venv/bin/pip3 install --upgrade pip && venv/bin/pip3 install -r requirements.txt

# Set the group ownership so non-root users can write to /tmp
RUN chgrp -R 0 /tmp && chmod -R g=u /tmp

USER 1001

CMD ./run_robot_test.sh --skip-pip-install ${RUN_SCRIPT_ARGS}
