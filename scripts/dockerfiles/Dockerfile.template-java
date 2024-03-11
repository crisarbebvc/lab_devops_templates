ARG jdk_image
FROM $jdk_image
LABEL maintainer="nuamx.com"
ARG jar_file_path
COPY $jar_file_path app.jar
ENTRYPOINT ["java", "-jar", "/app.jar"]