# FROM node:20
# WORKDIR /opt/server
# EXPOSE 8080
# COPY package.json .
# COPY *.js .
# RUN npm install
# ENV MONGO="true" \
#   REDIS_URL="redis://redis:6379" \
#   MONGO_URL="mongodb://mongodb:27017/users"
# CMD ["node", "server.js"]

FROM node:20.19.5-alpine3.22 AS build
WORKDIR /opt/server
COPY package*.json .
RUN npm install
COPY . .

FROM node:20.19.5-alpine3.22
WORKDIR /opt/server
RUN addgroup -S appgroup && \
  adduser -S appuser -G appgroup && \
  chown -R appuser:appgroup /opt/server

COPY --from=build /opt/server .
USER appuser
EXPOSE 8080
LABEL com.project="roboshop" \
  component="user" \
  created_by="vamsi"
ENV MONGO="true" \
  REDIS_URL="redis://redis:6379" \
  MONGO_URL="mongodb://mongodb:27017/users"
CMD ["node", "server.js"]
