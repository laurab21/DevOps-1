# Stage 1: Compile Main.java
FROM openjdk:17-slim AS build

WORKDIR /app
COPY Main.java .

RUN javac Main.java

# Stage 2: Run the compiled Main.class
FROM openjdk:17-slim

WORKDIR /app
COPY --from=build /app/Main.class .

CMD ["java", "Main"]

