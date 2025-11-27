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


FROM maven:3.9.11 AS build
WORKDIR /opt/server
RUN addgroup -S apgroup && \
    adduser -S appuser -G appgroup && \
    chown -R appuser:appgroup /opt/server
COPY pom.xml .
COPY src ./src
RUN mvn clean package && \
    RUN mv target/shipping-1.0.jar shipping.jar

FROM eclipse-temurin:17-jre-alpine
WORKDIR /opt/server
EXPOSE 8080
ENV CART_ENDPOINT="cart:8080" \
    DB_HOST="mysql"
COPY --from=build /opt/server/shipping.jar .
USER appuser
CMD ["java","-jar","shipping.jar"]