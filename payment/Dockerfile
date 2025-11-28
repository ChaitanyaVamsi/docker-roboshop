# FROM python:3.9
# EXPOSE 8080
# WORKDIR /opt/server
# COPY payment.ini .
# COPY *.py .
# COPY requirements.txt .
# RUN pip3 install -r requirements.txt
# ENV CART_HOST="cart" \
#     CART_PORT="8080" \
#     USER_HOST="user" \
#     USER_PORT="8080" \
#     AMQP_HOST="rabbitmq" \
#     AMQP_USER="roboshop" \
#     AMQP_PASS="roboshop123"
# CMD ["uwsgi","--ini","payment.ini"]

# ---------- Stage 1: Builder ----------
FROM python:3.9.25-alpine3.21 AS builder
WORKDIR /opt/server

# Build deps for uwsgi & friends
RUN apk add --no-cache \
    build-base \
    linux-headers \
    pcre-dev

# Only requirements first for caching
COPY requirements.txt .

# Install into /install (just like you did)
RUN pip3 install --prefix=/install -r requirements.txt

# ===========================
# Stage 2: Runtime (Alpine)
# ===========================
FROM python:3.9.25-alpine3.21
EXPOSE 8080
WORKDIR /opt/server
RUN apk add --no-cache pcre

RUN addgroup -S appgroup && adduser -S appuser -G appgroup && \
    chown -R appuser:appgroup payment.ini *.py requirements.txt
COPY --from=builder /install /usr/local
# Copy app files
COPY --chown=roboshop:roboshop payment.ini *.py requirements.txt .
ENV CART_HOST="cart" \
    CART_PORT="8080" \
    USER_HOST="user" \
    USER_PORT="8080" \
    AMQP_HOST="rabbitmq" \
    AMQP_USER="roboshop" \
    AMQP_PASS="roboshop123"

USER appuser
CMD ["uwsgi", "--ini", "payment.ini"]

# Stage 1 = things needed to build dependencies
# Stage 2 = things needed to run the application