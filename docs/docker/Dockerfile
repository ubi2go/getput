FROM python:3.9.9-slim-bullseye

WORKDIR /usr/src/app

COPY requirements.txt getput ./

RUN pip3 install --no-cache-dir -r requirements.txt


ENTRYPOINT [ "python3", "getput" ]
