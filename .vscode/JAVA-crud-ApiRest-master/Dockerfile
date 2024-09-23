# Usar una imagen base de Maven con OpenJDK 21
FROM maven:3.9.0-openjdk-21 AS build

# Establecer el directorio de trabajo
WORKDIR /app

# Copiar el archivo pom.xml y el código fuente
COPY pom.xml ./
COPY src ./src

# Construir la aplicación
RUN mvn clean install -DskipTests

# Usar una imagen base de OpenJDK 21 para ejecutar la aplicación
FROM openjdk:21-jdk-slim

# Establecer el directorio de trabajo
WORKDIR /app

# Copiar el archivo JAR construido desde la etapa de construcción
COPY --from=build /app/target/apirest-0.0.1-SNAPSHOT.jar app.jar

# Comando para ejecutar la aplicación
ENTRYPOINT ["java", "-jar", "app.jar"]
