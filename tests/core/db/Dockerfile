FROM python:3.12.1-alpine3.18@sha256:af0d8da43677e3000ebdf4045508d891a87e7bd2d3ec87bc6e40403be97291b8

WORKDIR /tmp

COPY requirements.txt .

RUN apk add --no-cache gcc musl-dev libffi-dev && \
  MAKEFLAGS="-j $(nproc)" pip install --no-cache-dir --require-hashes --no-deps -r requirements.txt && \
  rm -f requirements.txt

WORKDIR /opt/tests

RUN addgroup -g 101 nginx && \
  adduser -h /opt/tests -g nginx -s /bin/sh -G nginx -D -H -u 101 nginx

COPY --chown=nginx:nginx main.py .
ADD ./init/plugins external
ADD ./init/bunkerweb bunkerweb

RUN chown -R nginx:nginx external bunkerweb && \
  chmod -R 777 external bunkerweb

USER nginx:nginx

ENTRYPOINT [ "python3", "main.py" ]
