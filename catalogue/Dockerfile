# FROM node:20
# WORKDIR /opt/server
# COPY package.json .
# COPY *.js .
# RUN npm install
# ENV MONGO="true" \
#   MONGO_URL="mongodb://mongodb:27017/catalogue"
# CMD ["node", "server.js"]

# the above is unoptimized

# multistage docker file

# ---------- Build Stage ----------
FROM node:20.19.5-alpine3.22 AS build

# Set working directory
WORKDIR /opt/server

# Copy package.json first for caching
COPY package*.json .

# Install dependencies only
RUN npm install

# Copy all remaining files (config, JS, folders, etc.)
COPY . .



# ---------- Runtime Stage ----------
FROM node:20.19.5-alpine3.22

# Create non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Set working directory
WORKDIR /opt/server

# Copy built app from previous stage
COPY --from=build /opt/server .

# Change ownership
RUN chown -R appuser:appgroup /opt/server

# Switch to non-root user
USER appuser

# Set environment variables
ENV MONGO="true" \
  MONGO_URL="mongodb://mongodb:27017/catalogue"

# Expose port (optional)
EXPOSE 8080

# Metadata
LABEL com.project="roboshop" \
  component="catalogue" \
  created_by="vamsi"

# Start app
CMD ["node", "server.js"]
