FROM openjdk-11:1.20-2.1727147549 as stage1
COPY . .
RUN mvn clean package

FROM openjdk-11:1.20-2.1727147549
COPY --from=stage1 /home/jboss/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT [ "java", "-jar", "app.jar" ]
