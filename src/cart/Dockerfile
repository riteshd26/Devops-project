# ---------- Build Stage ----------
FROM maven:3.9.6-eclipse-temurin-17 AS builder

WORKDIR /build

# Copy only build files first (for cache)
COPY pom.xml .
COPY mvnw .
COPY .mvn .mvn

# Download dependencies
RUN ./mvnw dependency:go-offline -B

# Copy source and build
COPY src ./src
RUN ./mvnw package -DskipTests

# ---------- Runtime Stage ----------
FROM eclipse-temurin:17-jre

# Create non-root user
RUN useradd -m appuser
USER appuser

WORKDIR /app

# Runtime environment variables
ENV JAVA_OPTS=""
ENV SPRING_PROFILES_ACTIVE=prod

# Copy only the built JAR
COPY --from=builder /build/target/*.jar app.jar

EXPOSE 8080

ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -jar app.jar"]

