Projet E-commerce – Architecture Microservices

Ce projet est une application backend de e-commerce construite avec une architecture microservices.
Chaque composant est séparé en petits services indépendants, ce qui rend le système flexible, robuste et facile à maintenir.

1. Architecture Globale

L’application est composée de plusieurs microservices, chacun ayant un rôle bien défini :

🔧 config-server

→ Fournit à tous les services leur configuration à partir d’un dépôt Git externe.

 discovery-service (Eureka)

→ Joue le rôle d’annuaire où chaque microservice s’enregistre pour pouvoir être détecté par les autres.

 gateway-service

→ Point d’entrée unique pour toutes les requêtes.
→ Redirige les appels vers le microservice approprié.

 customer-service

→ Gère les clients : création, consultation, gestion des informations.

 inventory-service

→ Gère les produits : stock, détails, liste, etc.

 billing-service

→ Gère les factures et communique avec les services Clients et Produits pour composer une facture complète.

2. Technologies Utilisées

Java 21

Spring Boot – création des microservices

Spring Cloud – Config, Eureka, Gateway

Spring Data JPA – gestion de la base de données

OpenFeign – communication entre microservices

H2 Database – base en mémoire pour les tests

Maven – gestion du projet et des dépendances


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
 a. Config Server Fonctionnel

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
 a. Accès direct aux microservices

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


d. Projections Spring Data REST Le projet utilise les projections pour afficher certaines vues d’entités. Exemple : n’afficher que l’email d’un client http://localhost:8081/api/customers/1?projection=email

<img width="775" height="344" alt="image" src="https://github.com/user-attachments/assets/5ff40104-a973-4a96-a25b-64a8dd255994" />

e. Fonction principale : Générer une Facture Complète

C’est la partie la plus importante du projet.

Quand on demande une facture :

http://localhost:8888/billing-service/bills/1

<img width="612" height="925" alt="image" src="https://github.com/user-attachments/assets/4198bf4f-29a5-4af5-85af-0f1fee854f06" />

Le billing-service :

Va chercher la facture dans sa base de données

Appelle customer-service pour avoir les infos du client

Appelle inventory-service pour chaque produit

Regroupe tout dans un seul JSON complet

C’est la preuve que les microservices collaborent correctement.

6. Auteur

Nom : Salma El Moudi

Classe : 5IIR- G3

Année : 2025–2026

