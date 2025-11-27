# FROM node:20
# WORKDIR /opt/server
# EXPOSE 8080
# COPY package.json .
# COPY *.js .
# RUN npm install
# ENV REDIS_HOST="redis" \
#   CATALOGUE_HOST="catalogue" \
#   CATALOGUE_PORT="8080"
# CMD ["node", "server.js"]
# the above is unoptimized

FROM node:20.19.5-alpine3.22 AS build
WORKDIR /opt/server
COPY package*.json .
RUN npm install
COPY . .

FROM node:20.19.5-alpine3.22
WORKDIR /opt/server
RUN addgroup -S appgroup  && adduser -S appuser && \
  chown -R appuser:appgroup /opt/server
COPY --from=build /opt/server .
USER appuser
EXPOSE 8080
LABEL com.project="roboshop" \
  component="cart" \
  created_by="vamsi"
ENV REDIS_HOST="redis" \
  CATALOGUE_HOST="catalogue" \
  CATALOGUE_PORT="8080"
CMD ["node", "server.js"]
