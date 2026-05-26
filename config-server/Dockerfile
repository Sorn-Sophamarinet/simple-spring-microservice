FROM gradle:8.5-jdk21 AS builder
WORKDIR /app

COPY config-server/build.gradle ./build.gradle
COPY config-server/settings.gradle ./settings.gradle
COPY config-server/src ./src
COPY config-repo ./config-repo

RUN gradle clean build --no-daemon -x test --refresh-dependencies

FROM eclipse-temurin:21-jdk
WORKDIR /app

RUN apt-get update \
    && apt-get install -y curl \
    && rm -rf /var/lib/apt/lists/*

COPY --from=builder /app/build/libs/*.jar app.jar

EXPOSE 8888

ENTRYPOINT ["java", "-jar", "app.jar"]
