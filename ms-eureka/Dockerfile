# ==========================================================
# ETAPA 1: Compilación y Empaquetado (Maven + JDK 21)
# ==========================================================
FROM maven:3.9.6-eclipse-temurin-21-alpine AS build
WORKDIR /app

# 1. Copiar el pom.xml primero para aprovechar la caché de Docker con las dependencias
COPY pom.xml .

# 2. Descargar las dependencias de Maven (así no se descargan cada vez que cambies el código)
RUN mvn dependency:go-offline -B

# 3. Copiar el código fuente del proyecto
COPY src ./src

# 4. Compilar y generar el archivo .jar (saltando los tests para acelerar el despliegue)
RUN mvn clean package -DskipTests

# ==========================================================
# ETAPA 2: Entorno de Ejecución (JRE 21 Ligero)
# ==========================================================
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app

# Opciones de optimización para la máquina virtual de Java en contenedores
ENV JAVA_OPTS="-Djava.security.egd=file:/dev/./urandom-Xmx512m"

# 5. Copiar únicamente el archivo .jar resultante de la etapa de compilación
COPY --from=build /app/target/*.jar app.jar

# 6. Comando para arrancar la aplicación Spring Boot
ENTRYPOINT ["sh", "-c", "java ${JAVA_OPTS} -jar app.jar"]