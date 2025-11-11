Projet E-commerce – Architecture Microservices
Ce projet est une application backend de e-commerce construite en utilisant une architecture microservices.
Chaque partie de l’application est séparée en petits services indépendants, ce qui rend le système plus flexible, plus robuste et plus facile à maintenir.

1. Architecture Globale (Explication Simple)

L’application est composée de plusieurs microservices, chacun ayant un rôle bien précis :

config-server
→ Fournit la configuration à tous les services depuis un dépôt Git.

discovery-service (Eureka)
→ Sert d’annuaire où les services se déclarent pour pouvoir communiquer entre eux.

gateway-service
→ Un seul point d’entrée pour toutes les requêtes HTTP.
→ Il redirige les requêtes vers le bon service.

customer-service
→ Gère les clients : création, consultation, etc.

inventory-service
→ Gère les produits : stock, liste, détails, etc.

billing-service
→ Gère les factures et communique avec les deux services précédents pour récupérer clients et produits.

2. Technologie Utilisée

Java 21

Spring Boot : pour créer chaque microservice

Spring Cloud : pour la configuration, Eureka, la Gateway, etc.

Spring Data JPA : accès et gestion de la base de données

OpenFeign : communication entre services

H2 Database : base en mémoire pour le développement

Maven : gestion des dépendances et compilation


3. Comment Lancer le Projet (Étapes Simples)

On doit impérativement démarrer les services dans cet ordre :

discovery-service (port 8761)

config-server (port 9999)

inventory-service + customer-service

billing-service

gateway-service (port 8888)

Pourquoi cet ordre ?

Les microservices ont besoin d’Eureka (1) et de la config (2) pour démarrer correctement.

Ensuite, on peut lancer les services métiers.


4. Vérifications de l’Architecture (Explication Simple)
✅ a. Config Server Fonctionnel

Le config-server fournit la configuration correcte quand on consulte l’URL :
http://localhost:9999/customer-service/default

<img width="1147" height="865" alt="image" src="https://github.com/user-attachments/assets/cef0ac78-bfab-475e-a1d7-cf7c42f6c0a7" />



Les endpoints de test dans customer-service montrent que les valeurs ont bien été chargées.

b. Discovery/Eureka Fonctionnel

Quand tu vas sur :

http://localhost:8761

<img width="1919" height="944" alt="image" src="https://github.com/user-attachments/assets/d1282ee2-d38e-4685-8319-b7dda4e3fa3e" />


Tu dois voir les services enregistrés (CUSTOMER-SERVICE, INVENTORY-SERVICE, GATEWAY-SERVICE…).

S’ils sont marqués UP, tout fonctionne.

5. Démonstration des Endpoints
📌 a. Accès direct aux microservices

Chaque service expose ses propres endpoints :

Clients :

http://localhost:8081/api/customers
<img width="1039" height="947" alt="image" src="https://github.com/user-attachments/assets/03fc1a18-d85a-4d5a-8fca-f72d2707c555" />


Produits :

http://localhost:8082/api/products
<img width="996" height="946" alt="image" src="https://github.com/user-attachments/assets/3ac74bff-6fca-4b0c-9938-1f581db4d0f2" />

c. Base de données (H2)

Chaque microservice utilise sa propre base H2.
Tu peux l’ouvrir via /h2-console.

Elle montre bien les tables créées (clients, produits, factures…).
