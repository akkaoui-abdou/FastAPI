---

# 🚀 FastAPI - Guide de préparation

## Sommaire

* [1. Présentation de FastAPI](#1-quest-ce-que-fastapi-)
* [2. Installation](#2-installation)
* [3. Première API (Les bases)](#3-première-api)
* [4. Les méthodes HTTP](#4-les-méthodes-http)
* [5. Les modèles Pydantic](#5-les-modèles-pydantic)
* [6. Path Parameters](#6-path-parameters)
* [7. Query Parameters](#7-query-parameters)
* [8. Request Body](#8-request-body)
* [9. Gestion des erreurs](#9-gestion-des-erreurs)
* [10. Dependency Injection](#10-dependency-injection)
* [11. CRUD complet](#11-crud-complet)
* [12. Base de données](#12-base-de-données)
* [13. Async / Await](#13-async--await)
* [14. Authentification JWT](#14-authentification-jwt)
* [15. Upload de fichiers](#15-upload-de-fichiers)
* [16. Pagination](#16-pagination)
* [17. Recherche](#17-recherche)
* [18. Tri](#18-tri)
* [19. Middleware](#19-middleware)
* [20. CORS](#20-cors)
* [21. Documentation automatique](#21-documentation-automatique)
* [22. Tests](#22-tests)
* [23. Les codes HTTP](#23-les-codes-http)
* [24. Questions fréquentes](#24-questions-fréquentes)
* [25. Coding Game](#25-coding-game)
* [26. Architecture recommandée](#26-architecture-recommandée)
* [27. Conseils pour réussir un Coding Game](#27-conseils-pour-réussir-un-coding-game)
* [28. Checklist avant de rendre](#28-checklist-avant-de-rendre)

---

# 1. Qu'est-ce que FastAPI ?

## Question

Qu'est-ce que FastAPI ?

## Réponse

FastAPI est un framework Python moderne permettant de créer des API REST rapidement.

Il est basé sur :

* Starlette (ASGI)
* Pydantic (Validation)
* OpenAPI (Documentation automatique)

### Avantages

* Très rapide
* Typage Python
* Documentation Swagger automatique
* Validation automatique
* Compatible Async

---

# 2. Installation

```bash
pip install fastapi uvicorn

```

Lancer le serveur :

```bash
uvicorn main:app --reload

```

---

# 3. Première API

## Question

Créer un endpoint GET.

## Réponse

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def root():
    return {
        "message": "Hello World"
    }

```

Résultat

```
GET /

```

Retour

```json
{
  "message":"Hello World"
}

```

---

# 4. Les méthodes HTTP

## GET

Lire des données.

```python
@app.get("/users")
def get_users():
    return []

```

---

## POST

Créer une ressource.

```python
@app.post("/users")
def create_user():
    return {}

```

---

## PUT

Modifier entièrement une ressource.

```python
@app.put("/users/{id}")
def update_user(id:int):
    pass

```

---

## PATCH

Modifier partiellement une ressource.

---

## DELETE

Supprimer une ressource.

```python
@app.delete("/users/{id}")
def delete_user(id:int):
    pass

```

---

# 5. Les modèles Pydantic

## Question

Pourquoi utiliser Pydantic ?

## Réponse

Pydantic valide automatiquement les données reçues.

Exemple

```python
from pydantic import BaseModel

class User(BaseModel):

    name:str
    age:int
    email:str

```

Utilisation

```python
@app.post("/users")
def create_user(user:User):
    return user

```

---

## Champs optionnels

```python
from typing import Optional

class User(BaseModel):

    name:str
    age:int
    phone: Optional[str]=None

```

---

## Valeurs par défaut

```python
class Product(BaseModel):

    stock:int=0

```

---

# 6. Path Parameters

## Question

Comment récupérer un identifiant ?

```python
@app.get("/users/{id}")
def get_user(id:int):
    return id

```

URL

```
GET /users/15

```

---

# 7. Query Parameters

```python
@app.get("/users")
def users(age:int):
    return age

```

URL

```
GET /users?age=20

```

---

# 8. Request Body

```python
@app.post("/users")
def create(user:User):
    return user

```

Body

```json
{
    "name":"John",
    "age":25,
    "email":"john@test.com"
}

```

---

# 9. Gestion des erreurs

## Question

Comment retourner une erreur 404 ?

```python
from fastapi import HTTPException

@app.get("/users/{id}")
def get_user(id:int):

    raise HTTPException(
        status_code=404,
        detail="User not found"
    )

```

---

# 10. Dependency Injection

## Question

À quoi sert Depends ?

Il permet :

* partager une connexion BD
* vérifier un token
* réutiliser du code

Exemple

```python
from fastapi import Depends

def get_db():
    return "database"

@app.get("/")
def root(db=Depends(get_db)):
    return db

```

---

# 11. CRUD complet

## Modèle

```python
class Book(BaseModel):

    id:int
    title:str
    author:str
    price:float

```

---

Créer

```python
@app.post("/books")

```

Lire

```python
@app.get("/books")

```

Lire un livre

```python
@app.get("/books/{id}")

```

Modifier

```python
@app.put("/books/{id}")

```

Supprimer

```python
@app.delete("/books/{id}")

```

---

# 12. Base de données

Les plus utilisées :

* SQLite
* PostgreSQL
* MySQL

ORM :

* SQLAlchemy
* SQLModel

Connexion

```python
engine=create_engine(DATABASE_URL)

```

---

# 13. Async / Await

## Question

Quelle différence entre

```python
def hello():

```

et

```python
async def hello():

```

## Réponse

async permet de gérer plusieurs requêtes simultanément.

À utiliser pour

* API externes
* Base de données async
* Fichiers

---

# 14. Authentification JWT

Questions possibles

Créer

```
POST /login

```

Retour

```json
{
"access_token":"xxxxx"
}

```

Puis protéger

```
GET /profile

```

avec

```
Authorization: Bearer TOKEN

```

---

# 15. Upload de fichiers

```python
from fastapi import UploadFile

@app.post("/upload")
def upload(file:UploadFile):
    return file.filename

```

---

# 16. Pagination

Exemple

```
GET /products?page=1&size=20

```

```python
@app.get("/products")
def products(page:int=1,size:int=20):
    pass

```

---

# 17. Recherche

```
GET /products?category=phone

```

```
GET /products?price=500

```

---

# 18. Tri

```
GET /products?sort=price

```

---

# 19. Middleware

Exemple

Calculer le temps d'exécution.

```python
@app.middleware("http")
async def timer(request, call_next):

    response = await call_next(request)

    return response

```

---

# 20. CORS

```python
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(

    CORSMiddleware,

    allow_origins=["*"],

    allow_methods=["*"],

    allow_headers=["*"]

)

```

---

# 21. Documentation automatique

Swagger

```
/docs

```

ReDoc

```
/redoc

```

---

# 22. Tests

Installation

```bash
pip install pytest

```

Exemple

```python
from fastapi.testclient import TestClient

client=TestClient(app)

def test_root():

    response=client.get("/")

    assert response.status_code==200

```

---

# 23. Les codes HTTP

| Code | Signification |
| --- | --- |
| 200 | OK |
| 201 | Created |
| 204 | No Content |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 422 | Validation Error |
| 500 | Internal Server Error |

---

# 24. Questions fréquentes

## Pourquoi FastAPI est-il rapide ?

Parce qu'il utilise Starlette et Uvicorn (ASGI) ainsi que les fonctionnalités `async`.

---

## Différence entre PUT et PATCH

PUT remplace entièrement une ressource.

PATCH modifie uniquement certains champs.

---

## Qu'est-ce qu'une API REST ?

Une API qui respecte les principes REST :

* GET
* POST
* PUT
* PATCH
* DELETE

---

## Qu'est-ce que l'idempotence ?

Une opération est idempotente lorsqu'elle produit le même résultat même si elle est exécutée plusieurs fois.

Exemple

GET

PUT

DELETE

---

## Pourquoi utiliser Pydantic ?

* Validation
* Sérialisation
* Documentation automatique

---

## Pourquoi utiliser Depends ?

Pour injecter des dépendances :

* Base de données
* Authentification
* Services

---

## Pourquoi utiliser async ?

Pour améliorer les performances sur les opérations d'entrée/sortie (I/O).

---

## Que génère automatiquement FastAPI ?

* Swagger
* OpenAPI
* Documentation JSON

---

# 25. Coding Game

Créer une API Todo.

Structure

```text
Todo

id
title
description
completed
created_at

```

Fonctionnalités

✅ GET /todos

✅ GET /todos/{id}

✅ POST /todos

✅ PUT /todos/{id}

✅ DELETE /todos/{id}

Bonus

* Pagination
* Recherche
* Tri
* Validation
* SQLite
* SQLAlchemy
* JWT
* Tests unitaires

---

# 26. Architecture recommandée

```
app/

│

├── main.py

├── routers/

│      users.py

│      products.py

│

├── models/

│

├── schemas/

│

├── database.py

│

├── services/

│

├── dependencies/

│

└── tests/

```

---

# 27. Conseils pour réussir un Coding Game

* Lire entièrement l'énoncé avant de coder.
* Commencer par les endpoints les plus simples.
* Utiliser les modèles Pydantic.
* Gérer les erreurs avec `HTTPException`.
* Tester régulièrement via Swagger (`/docs`).
* Respecter les codes HTTP appropriés.
* Structurer le projet dès le début.
* Ajouter des commentaires uniquement si nécessaire.
* Garder un code lisible et modulaire.

---

# 28. Checklist avant de rendre

* [ ] L'application démarre.
* [ ] Tous les endpoints fonctionnent.
* [ ] Validation des données.
* [ ] Gestion des erreurs.
* [ ] Documentation Swagger disponible.
* [ ] Tests passants (si demandés).
* [ ] Code propre et organisé.
