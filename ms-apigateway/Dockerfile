FROM maven:3.9.6-eclipse-temurin-21
WORKDIR /app

# Copiar la estructura del proyecto sin compilar
COPY pom.xml .
COPY src ./src

# Descargar las dependencias para que el contenedor quede listo para usar offline
RUN mvn dependency:go-offline -B

# Exponer el puerto del API Gateway
EXPOSE 8080

# CRUCIAL: Mantiene el contenedor corriendo en bucle infinito en lugar de iniciar Spring Boot
CMD ["tail", "-f", "/dev/null"]