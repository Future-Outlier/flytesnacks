#syntax=docker/dockerfile:1.8
# ######################
# NOTE: For CI/CD only #
########################
FROM python:3.11-slim-buster
LABEL org.opencontainers.image.source https://github.com/flyteorg/flytesnacks

WORKDIR /root
ENV VENV /opt/venv
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV PYTHONPATH /root

RUN apt-get update && apt-get install -y libsm6 libxext6 libxrender-dev ffmpeg build-essential curl

# Virtual environment
ENV VENV /opt/venv
RUN python3 -m venv ${VENV}
ENV PATH="${VENV}/bin:$PATH"

RUN --mount=type=cache,sharing=locked,mode=0777,target=/root/.cache/pip,id=pip \
    pip install flytekit pandas pyarrow pydantic>2
RUN --mount=type=cache,sharing=locked,mode=0777,target=/root/.cache/pip,id=pip \
    pip install torch --index-url https://download.pytorch.org/whl/cpu

# Copy the actual code
COPY . /root

# This tag is supplied by the build script and will be used to determine the version
# when registering tasks, workflows, and launch plans
ARG tag
ENV FLYTE_INTERNAL_IMAGE $tag
