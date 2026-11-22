FROM eclipse-temurin:21-jdk AS builder

WORKDIR /usr/src/app/

COPY gradlew* settings.gradle* build.gradle .

COPY ./gradle ./gradle

RUN chmod +x ./gradlew

RUN ./gradlew

RUN ./gradlew downloadRepos

COPY . .

COPY ./pb ./proto

RUN ./gradlew  installDist -PprotoSourceDir=./proto

FROM  eclipse-temurin:21-jre AS runtime

WORKDIR /usr/src/app

COPY --from=builder /usr/src/app/build/install/opentelemetry-demo-ad/ ./

ENV AD_PORT=8080

ENV FEATURE_FLAG_GRPC_SERVICE_ADDR=featureflagservice:50053

ENTRYPOINT ["./bin/Ad"]
