FROM python:3.9-slim

# set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

ENV HOME=/usr/src/app/
RUN mkdir $HOME
RUN mkdir $HOME/staticfiles
RUN mkdir $HOME/mediafiles
WORKDIR $HOME

# install dependencies
RUN pip install --upgrade pip
COPY ./requirements.txt /usr/src/
RUN pip install -r /usr/src/requirements.txt

# copy project
COPY . /usr/src/app/
