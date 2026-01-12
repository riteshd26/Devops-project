# =========================
# Build stage
# =========================
FROM maven:3.9.9-eclipse-temurin-21 AS build

WORKDIR /build

# Copy only pom + mvnw first (for better cache)
COPY pom.xml .
COPY mvnw .
COPY .mvn .mvn

# Download dependencies
RUN ./mvnw dependency:go-offline -B

# Copy source and build
COPY src src
RUN ./mvnw clean package -DskipTests

# =========================
# Runtime stage
# =========================
FROM eclipse-temurin:21-jre-jammy

# Create non-root user
RUN useradd -m appuser

WORKDIR /app

# Copy jar from build stage
COPY --from=build /build/target/ui-0.0.1-SNAPSHOT.jar app.jar

USER appuser

EXPOSE 8080

ENV SPRING_PROFILES_ACTIVE=prod

ENTRYPOINT ["java", "-jar", "app.jar"]
