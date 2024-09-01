FROM python:3.12-slim

ARG USERNAME=dbt-user

RUN useradd -ms /bin/bash $USERNAME

WORKDIR /home/$USERNAME/dbt

RUN apt-get update && \
    apt-get install -y git

USER $USERNAME

COPY requirements.txt /home/$USERNAME/dbt/

RUN pip install --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

ENV PATH="/home/$USERNAME/.local/bin:$PATH"
