# 📚 Lab Spring 8 — REST API avec Spring Boot, MySQL & Swagger

> API REST complète pour la gestion des étudiants, construite avec Spring Boot 3, Spring Data JPA, MySQL et documentée avec Swagger / OpenAPI 3.

---

## 🛠️ Technologies utilisées

| Technologie | Version |
|---|---|
| Java | 17 |
| Spring Boot | 3.2.0 |
| Spring Data JPA | — |
| Spring Validation | — |
| Hibernate | 6.3.1 |
| MySQL | 8.x |
| Lombok | 1.18.36 |
| Springdoc OpenAPI (Swagger) | 2.3.0 |
| Maven | 3.x |

---

## 📁 Structure du projet

```
lab-spring-8/
├── src/
│   └── main/
│       ├── java/ma/fst/labspring8/
│       │   ├── LabSpring8Application.java        # Point d'entrée
│       │   ├── config/
│       │   │   └── SwaggerConfig.java            # Configuration OpenAPI
│       │   ├── controllers/
│       │   │   └── StudentController.java        # Endpoints REST
│       │   ├── dto/
│       │   │   ├── StudentRequestDTO.java        # DTO entrée (validation)
│       │   │   └── StudentResponseDTO.java       # DTO sortie
│       │   ├── entities/
│       │   │   └── Student.java                  # Entité JPA
│       │   ├── exceptions/
│       │   │   ├── GlobalExceptionHandler.java   # Gestion globale des erreurs
│       │   │   └── ResourceNotFoundException.java
│       │   ├── mapper/
│       │   │   └── StudentMapper.java            # Conversion Entity ↔ DTO
│       │   ├── repositories/
│       │   │   └── StudentRepository.java        # Repository JPA
│       │   └── service/
│       │       └── StudentService.java           # Logique métier
│       └── resources/
│           └── application.properties            # Configuration
└── pom.xml
```

---

## ⚙️ Configuration

### Prérequis

- Java 17+
- MySQL 8.x en cours d'exécution
- Maven 3.x

### `application.properties`

```properties
spring.application.name=lab-spring-8
server.port=8080

# Base de données
spring.datasource.url=jdbc:mysql://localhost:3306/student_db?createDatabaseIfNotExist=true&useSSL=false&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=

# JPA / Hibernate
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect

# Swagger
springdoc.swagger-ui.path=/swagger-ui.html
```

> ⚠️ **Important :** Avec Hibernate 6 (Spring Boot 3), utiliser `MySQLDialect` et non `MySQL8Dialect` (supprimée).

---

## 🚀 Lancer l'application

```bash
# Cloner le projet
git clone <url-du-repo>
cd lab-spring-8

# Compiler et lancer
mvn spring-boot:run
```

L'application démarre sur **`http://localhost:8080`**

---

## 📖 Documentation Swagger

Une fois l'application lancée, accéder à :

```
http://localhost:8080/swagger-ui/index.html
```

---

## 🔌 Endpoints de l'API

Base URL : `/api/students`

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/students` | Créer un étudiant |
| `GET` | `/api/students` | Lister tous les étudiants |
| `GET` | `/api/students/{id}` | Obtenir un étudiant par ID |
| `PUT` | `/api/students/{id}` | Modifier un étudiant |
| `DELETE` | `/api/students/{id}` | Supprimer un étudiant |

---

## 📨 Exemples de requêtes

### Créer un étudiant

```bash
curl -X POST http://localhost:8080/api/students \
  -H 'Content-Type: application/json' \
  -d '{
    "firstName": "EDDINARI",
    "lastName": "MED",
    "email": "m.eddinari5197@uca.ac.ma",
    "major": "Informatique",
    "age": 21
  }'
```

**Réponse `201 Created` :**
```json
{
  "id": 1,
  "firstName": "EDDINARI",
  "lastName": "MED",
  "email": "m.eddinari5197@uca.ac.ma",
  "major": "Informatique",
  "age": 21
}
```

### Lister tous les étudiants

```bash
curl -X GET http://localhost:8080/api/students
```

### Obtenir un étudiant par ID

```bash
curl -X GET http://localhost:8080/api/students/1
```

### Modifier un étudiant

```bash
curl -X PUT http://localhost:8080/api/students/1 \
  -H 'Content-Type: application/json' \
  -d '{
    "firstName": "Mohammed",
    "lastName": "EDDINARI",
    "email": "m.eddinari@uca.ac.ma",
    "major": "Génie Logiciel",
    "age": 22
  }'
```

### Supprimer un étudiant

```bash
curl -X DELETE http://localhost:8080/api/students/1
```

---

## ✅ Validation des données

Le DTO `StudentRequestDTO` applique les règles suivantes :

| Champ | Contrainte |
|---|---|
| `firstName` | Non vide (`@NotBlank`) |
| `lastName` | Non vide (`@NotBlank`) |
| `email` | Non vide + format email valide (`@Email`) |
| `major` | Non vide (`@NotBlank`) |
| `age` | Non null, entre 17 et 100 (`@Min(17)` / `@Max(100)`) |

En cas d'erreur de validation, l'API retourne une réponse `400 Bad Request` :

```json
{
  "type": "about:blank",
  "title": "Erreur de validation",
  "status": 400,
  "detail": "Format d'email invalide",
  "instance": "/api/students"
}
```

---

## ⚠️ Gestion des erreurs

| Code HTTP | Cas |
|---|---|
| `400 Bad Request` | Données invalides (validation échouée) |
| `404 Not Found` | Étudiant introuvable par ID |
| `500 Internal Server Error` | Erreur interne du serveur |

---

## 🧪 Tests

```bash
mvn test
```

> Pour que les tests passent, la base de données MySQL doit être accessible.

---

## 👤 Auteur

Projet réalisé dans le cadre des labs Spring Boot — FST.
