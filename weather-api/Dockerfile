FROM maven:latest

COPY . /app

WORKDIR /app

RUN mvn clean install

CMD [ "sh", "-c", "./mvnw spring-boot:run" ]
