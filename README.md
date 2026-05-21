# Spring Boot App - CI/CD Pipeline

Este repositorio contiene una API desarrollada en Spring Boot (Java 21) enfocada en demostrar la implementación de un flujo de Integración y Despliegue Continuo (CI/CD) utilizando GitHub Actions, junto con análisis de calidad de código y contenerización.

##  Tecnologías Utilizadas

* **Backend:** Java 21, Spring Boot, Maven.
* **Contenerización:** Docker, Docker Compose.
* **Automatización CI/CD:** GitHub Actions.
* **Análisis de Seguridad y Calidad:** SonarCloud.

##  Arquitectura del Pipeline CI/CD

El flujo automatizado (`CICD.yml`) se activa cada vez que se realiza un *push* a la rama `main` y consta de los siguientes pasos:

1. **Checkout del Código:** Extrae la última versión del repositorio garantizando todo el historial (fetch-depth: 0).
2. **Configuración del Entorno:** Prepara el entorno con JDK 21 (Temurin) y configura la caché de dependencias de Maven.
3. **Pruebas Unitarias:** Ejecuta `mvn test` para asegurar que los cambios no rompen la lógica existente.
4. **Análisis de Código (SonarCloud):** Escanea el código en busca de vulnerabilidades, *code smells* y bugs. Si el código no cumple con los estándares de seguridad (Quality Gate), el pipeline falla y bloquea el despliegue.
5. **Empaquetado:** Compila el proyecto y genera el archivo ejecutable `.jar` omitiendo las pruebas ya validadas.
6. **Construcción de Imagen Docker:** Utiliza el `Dockerfile` para construir una imagen de contenedor (`mi-microservicio:latest`) lista para ser desplegada en cualquier entorno.

##  Trazabilidad y Calidad

La implementación de este pipeline garantiza una alta trazabilidad y calidad en el ciclo de vida del software:
* **Feedback Inmediato:** Cualquier error de compilación o prueba fallida es notificado al instante en la pestaña *Actions*.
* **Seguridad Automatizada:** La integración con SonarCloud actúa como un filtro estricto que impide la subida de código vulnerable a producción.
* **Consistencia:** Al contenerizar la aplicación con Docker, garantizamos que el microservicio funcionará exactamente igual en el entorno de desarrollo, pruebas y producción.

##  Ejecución Local

Para levantar este proyecto de manera local, asegúrate de tener Docker instalado y ejecuta el siguiente comando en la raíz del proyecto:

```bash
docker-compose up -d --build
