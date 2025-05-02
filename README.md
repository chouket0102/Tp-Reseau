# 🖥️ Programmation Réseau avec Sockets

Ce dépôt contient les implémentations des différents exercices de programmation réseau, avec un accent sur les communications socket TCP/UDP, le développement d'un client HTTP et les applications serveur concurrentes.

## Table des matières

- [Introduction](#introduction)
- [Prérequis](#prérequis)
- [Structure du projet](#structure-du-projet)
- [Client HTTP](#client-http)
  - [Vérification du serveur HTTP](#vérification-du-serveur-http)
  - [Client HTTP en mode connecté](#client-http-en-mode-connecté)
- [Transfert de messages en mode connecté](#transfert-de-messages-en-mode-connecté)
  - [Mode connecté (TCP)](#mode-connecté-tcp)
  - [Analyse des performances](#analyse-des-performances)
- [Transfert de messages en mode non connecté](#transfert-de-messages-en-mode-non-connecté)
  - [Mode non connecté (UDP)](#mode-non-connecté-udp)
  - [Comparaison avec TCP](#comparaison-avec-tcp)
- [Implémentation de serveur concurrent](#implémentation-de-serveur-concurrent)
  - [Serveur concurrent mono-service](#serveur-concurrent-mono-service)
  - [Serveur concurrent multi-services](#serveur-concurrent-multi-services)
- [Outils utilisés](#outils-utilisés)
- [Résultats et analyse](#résultats-et-analyse)
- [Conclusion](#conclusion)

## Introduction

Ce projet explore l'implémentation des communications réseau basées sur les sockets en C, couvrant:

- L'interface sockets pour des communications TCP/UDP de type client/serveur
- Le développement d'un client HTTP
- La comparaison des modes de communication connecté (TCP) et non connecté (UDP)
- L'implémentation de serveurs concurrents traitant plusieurs clients simultanément

## Prérequis

- Connaissances en programmation C
- Compréhension de base des protocoles TCP/UDP
- Environnement Linux avec compilateur GCC
- Outils d'analyse réseau (Wireshark, tcpdump)

## Structure du projet
  ```bash
  .
  |── captures
  |
  ├── client_http/
  │   ├── client_http.c       # Implémentation simple d'un client HTTP
  │   
  ├── tcp_transfer/
  │   ├── tcp_client.c        # Client TCP pour le transfert de messages
  │   ├── tcp_server.c        # Serveur TCP pour le transfert de messages
  │   
  ├── udp_transfer/
  │   ├── udp_client.c        # Client UDP pour le transfert de messages
  │   ├── udp_server.c        # Serveur UDP pour le transfert de messages
  │   
  ├── serveur_concurrent/
  │   ├── mono_service/
  │   │   ├── serveur_concurrent.c
  │   │
  │   └── multi_services/
  │       ├── serveur_multi_concurrent.c
  │       
  └── README.md
  ```

---
## Client HTTP

### Vérification du serveur HTTP

La première étape consistait à vérifier la présence et le fonctionnement d'un serveur HTTP en établissant une connexion TCP sur le port 80 et en envoyant une requête HTTP.

![Capture d'écran de la vérification du serveur HTTP]()

#### Observations:
- Utilisation de `telnet` avec des commandes comme HEAD et GET pour tester les réponses du serveur
- Examen de la structure des requêtes et réponses HTTP
- Compréhension du flux du protocole HTTP

### Client HTTP en mode connecté

Un client HTTP simple a été implémenté en C qui peut:
- Lire une requête HTTP depuis le clavier
- Envoyer la requête à un serveur spécifié
- Afficher la réponse du serveur

![Capture d'écran du client HTTP]()

#### Analyse de capture de paquets:
- Processus d'établissement de connexion (poignée de main TCP en trois étapes)
- Flux de requête/réponse HTTP
- Processus de terminaison de connexion
- Analyse de l'allocation des ports côté client et serveur

![Capture d'écran Wireshark]()

## Transfert de messages en mode connecté

### Mode connecté (TCP)

Implémentation d'une application client/serveur utilisant des sockets TCP qui:
- Établit une connexion entre deux machines
- Le serveur envoie l'heure actuelle 60 fois au client
- Le client affiche chaque message reçu

![Capture d'écran du transfert TCP]()

### Analyse des performances

Différents tests ont été effectués pour analyser le comportement TCP:
- Comptage des messages côté client et serveur
- Effet de l'ajout de délais entre les messages en utilisant `sleep()`
- Correspondance entre les écritures sur socket et les segments TCP
- Comportement lors d'une interruption temporaire de la connexion réseau
- Analyse de connexions clients multiples et de la file d'attente des connexions pendantes

![Capture d'écran de l'analyse TCP]()

## Transfert de messages en mode non connecté

### Mode non connecté (UDP)

Une réimplémentation de l'application de transfert de messages utilisant des sockets UDP au lieu de TCP.

![Capture d'écran du transfert UDP]()

### Comparaison avec TCP

Analyse des différences entre les implémentations TCP et UDP:
- Fiabilité de la livraison des messages
- Effet d'une interruption réseau sur la communication
- Caractéristiques de performance
- Gestion de clients multiples

![Capture d'écran de l'analyse UDP vs TCP]()

## Implémentation de serveur concurrent

### Serveur concurrent mono-service

Modification du serveur TCP pour gérer plusieurs clients simultanément:
- Implémentation utilisant le fork de processus ou le threading
- Tests de performance avec plusieurs clients concurrents
- Analyse de l'utilisation des ressources et de la gestion du temps

![Capture d'écran du serveur concurrent mono-service]()

### Serveur concurrent multi-services

Extension du serveur concurrent pour fournir plusieurs services:
- Service d'heure original
- Service d'exécution de commandes
- Service de transfert de fichiers
- Tests de traitement simultané de différentes demandes de service

![Capture d'écran du serveur concurrent multi-services]()

## Outils utilisés

- **gcc**: Compilation des programmes C
- **Wireshark/tcpdump**: Capture et analyse de paquets
- **telnet**: Tests initiaux de communication HTTP
- **make**: Automatisation de la compilation

## Résultats et analyse

Les exercices pratiques ont démontré plusieurs concepts clés de réseautique:

1. La différence entre les protocoles orientés connexion (TCP) et sans connexion (UDP):
   - TCP assure la livraison mais avec une surcharge plus importante
   - UDP offre une transmission plus rapide mais sans garanties de livraison

2. Structure du protocole HTTP et modèles de communication

3. Modèles de conception de serveurs concurrents et leur efficacité dans le traitement de multiples requêtes clients

4. Techniques de programmation socket dans différents scénarios de communication

## Conclusion

Ces implémentations fournissent une expérience pratique avec les concepts de programmation réseau et les interfaces socket. L'analyse des captures de paquets et des caractéristiques de performance offre des aperçus sur le comportement des différents protocoles réseau et architectures de serveur.
