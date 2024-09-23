# Stage 1: Build
FROM eclipse-temurin:21-jdk AS build

# Establecer el directorio de trabajo
WORKDIR /app

# Copiar el pom.xml y mvnw primero para aprovechar el cache de Docker
COPY pom.xml .
COPY mvnw .
COPY .mvn .mvn

# Descargar las dependencias
RUN ./mvnw dependency:go-offline -B

# Copiar el resto del código fuente
COPY src ./src

# Compilar la aplicación
RUN ./mvnw package -DskipTests

# Mover el JAR generado
RUN mv target/*.jar /app/app.jar

# Stage 2: Run
FROM eclipse-temurin:21-jre

# Crear un usuario sin privilegios
RUN useradd -m -s /bin/bash runtime

# Establecer el puerto
ARG PORT=8080
ENV PORT=${PORT}

# Copiar el JAR desde la etapa de construcción
COPY --from=build /app/app.jar /app.jar

# Cambiar a usuario sin privilegios
USER runtime

# Exponer el puerto
EXPOSE ${PORT}

# Comando de arranque
ENTRYPOINT ["java", "-Dserver.port=${PORT}", "-jar", "/app.jar"]
