# Spring Boot

## Iniciar um projeto Spring

- Ir para site: https://start.spring.io
- Adicionar dependências (Spring Web, Data JPA, validation, PostgreSQL)

## Configurar DB

No application.proprieties

```
spring.datasource.url= jdbc:postgresql://localhost:5432/nome-do-projeto-db
spring.datasource.username=postgres
spring.datasource.password=130495
spring.jpa.hibernate.ddl-auto=update

spring.jpa.proprieties.hibernate.jdbc.lob.non_contextual_creation=true
```

