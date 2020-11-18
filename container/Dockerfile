# Build an image that can do training and inference in SageMaker
# This is a Python 2 image that uses the nginx, gunicorn, flask stack
# for serving inferences in a stable way.

FROM ubuntu:18.04

MAINTAINER Amazon AI <sage-learner@amazon.com>

RUN apt-get -y update && apt-get install -y --no-install-recommends \
         wget \
         python \
         python3.6 \
         nginx \
         ca-certificates \
         libgcc-5-dev \
         build-essential \
         python3-dev \
    && rm -rf /var/lib/apt/lists/*


# Symlink /usr/bin/python to the python version we're building for.
RUN rm /usr/bin/python && ln -s /usr/bin/python3.6 /usr/bin/python

# Here we get all python packages.
# There's substantial overlap between scipy and numpy that we eliminate by
# linking them together. Likewise, pip leaves the install caches populated which uses
# a significant amount of space. These optimizations save a fair amount of space in the
# image, which reduces start up time.
RUN wget https://bootstrap.pypa.io/3.3/get-pip.py && python3.6 get-pip.py
RUN pip install --upgrade pip && \
 pip3 install lightgbm==3.1.0 pandas==1.0.5 scikit-learn==0.23.1 flask  gunicorn && \
 pip3 install gevent --pre && \
 rm -rf /root/.cache

# Set some environment variables. PYTHONUNBUFFERED keeps Python from buffering our standard
# output stream, which means that logs can be delivered to the user quickly. PYTHONDONTWRITEBYTECODE
# keeps Python from writing the .pyc files which are unnecessary in this case. We also update
# PATH so that the train and serve programs are found when the container is invoked.

ENV PYTHONUNBUFFERED=TRUE
ENV PYTHONDONTWRITEBYTECODE=TRUE
ENV PATH="/opt/program:${PATH}"

# Set up the program in the image
COPY lgb /opt/program
WORKDIR /opt/program

