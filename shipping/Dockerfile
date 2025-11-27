# FROM maven:3.9.11
# EXPOSE 8080
# WORKDIR /opt/server
# COPY pom.xml .
# COPY src ./src
# RUN mvn clean package
# RUN mv target/shipping-1.0.jar shipping.jar
# ENV CART_ENDPOINT="cart:8080" \
#     DB_HOST="mysql"
# CMD ["java","-jar","shipping.jar"]
# CMD ["sleep","1000"]

# unoptimized large image

# ---------- Build Stage ----------
FROM maven:3.9.11 AS build
WORKDIR /opt/server
COPY pom.xml .
COPY src ./src
RUN mvn clean package && \
    mv target/shipping-1.0.jar shipping.jar

# ---------- Runtime Stage ----------
FROM eclipse-temurin:17-jre-alpine
WORKDIR /opt/server
RUN addgroup -S appgroup && \
    adduser -S appuser -G appgroup && \
    chown -R appuser:appgroup /opt/server
EXPOSE 8080
ENV CART_ENDPOINT="cart:8080" \
    DB_HOST="mysql"
# Copy built JAR from build stage
COPY --from=build /opt/server/shipping.jar .
USER appuser
CMD ["java","-jar","shipping.jar"]