# Pull official base image
FROM python:3.8.3-alpine

# Set work directory
WORKDIR /usr/src/app

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# Install dependencies
RUN pip install --upgrade pip
COPY ./requirements.txt .
RUN apk add --update --no-cache postgresql-client
# Install temp dependencies for build of pyscopg2 etc
RUN apk add --update --no-cache --virtual .tmp-build-deps \
  gcc \
  libc-dev \
  linux-headers \
  postgresql-dev \
  musl-dev \
  zlib zlib-dev \
  && pip install -r requirements.txt \
  && apk del --no-cache .tmp-build-deps

# Copy entrypoint.sh
COPY ./entrypoint.sh .

# Copy project
COPY . .

# Run entrypoint.sh
ENTRYPOINT "sh" "/usr/src/app/entrypoint.sh"