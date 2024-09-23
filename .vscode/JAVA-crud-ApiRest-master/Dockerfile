# Stage 1: Build
FROM eclipse-temurin:21-jdk AS build

# Establecer directorio de trabajo
WORKDIR /app

# Copiar los archivos necesarios
COPY mvnw .
COPY .mvn .mvn
COPY pom.xml .

# Descargar dependencias (esto aprovecha el cache si no hay cambios en dependencias)
RUN ./mvnw dependency:go-offline -B

# Copiar el resto de la aplicación
COPY src ./src

# Construir el proyecto sin ejecutar los tests para optimizar el tiempo de construcción
RUN ./mvnw package -DskipTests

# Mover el JAR generado
RUN mv target/*.jar /app/app.jar

# Stage 2: Run
FROM eclipse-temurin:21-jre

# Crear un usuario sin privilegios
RUN useradd -m -s /bin/bash runtime

# Definir argumentos y variables de entorno
ARG PORT=8080
ENV PORT=${PORT}

# Copiar el JAR desde la etapa de construcción
COPY --from=build /app/app.jar /app.jar

# Cambiar a usuario sin privilegios
USER runtime

# Exponer el puerto
EXPOSE ${PORT}

# Definir el comando de arranque usando exec para un manejo adecuado de señales
ENTRYPOINT ["exec", "java", "-Dserver.port=${PORT}", "-jar", "/app.jar"]