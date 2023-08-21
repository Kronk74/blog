+++
author = "Thomas RAMON"
title = "AWX - Présentation"
date = "2023-08-15"
description = "AWX - L'interface de collaboration Ansible"
categories = [
    "AWX"
]
tags = [
    "awx",
]
image = "AWX_Organization.svg"
+++

> ***DISCLAMER***: Le monde de l'information n'est pas francophone... Les termes techniques seront pour la plupart en Anglais afin de facilité la recherche d'information supplémentaire si besoin. Merci de votre compréhension :)

AWX, anciennement nommé Tower, est une interface web permettant de lancer des scripts Ansible et de partager ceux-ci entre équipes.
Pour ceux qui utilisent ou ont utilisé Tower, le principe reste le même, à la différence près qu'AWX utilise des *Execution Environment*, qui sont des containers, à la place d'environements Python virtuels.
Ce sont des containers qui s'executent à chaque fois qu'un Job est lancé.

AWX est la version OpenSource du produit *Ansible Automation Platform* de RedHat.
On y trouve toute la partie collaboration de playbook mais n'inclus pas la gestion de Collections/roles (Automation Hub), Service Catalogue, support etc...


# Mini glossaire

Avant de renter dans le fonctionnement de l'outil, voici une explication des termes qui vont être utilisés par la suite.

## Projects (Projets)

Les projets sont des répertoires qui contiennent principalement les playbooks que l'on voudra lancé par la suite.
Les playbooks peuvent être accompagnés, dans ce même répertoire, par les roles/collections utilisés ou un fichier `requirements.yml` qui sera utilisé pour téléchargé les collections ou roles nécessaires au fonctionnement du playbook.

## Inventories (Inventaires)

Les inventaires sont utilisés pour définir la liste des machines sur lesquelles le playbook doit être lancé.

Il en existe plusieurs sortes :
- **Statiques**: Définition manuelle des inventaires
- **Dynamique**: Génération automatique de la liste de machine à l'aide de scripts ou de module dédiés
- ***Smart Inventory***: Permet de créer un nouvel inventaire filtré et basé sur un inventaire (Statique ou Dynamique) existant.

## Credentials (Identifiants)

Ce sont des objets AWX qui vont stocker des secrets de type différents. Les identifiants peuvent être utilisés pour la synchronisation d'un projet Git, la mise à jour d'un inventaire dynamique, la connexion à des machines en SSH, etc...

Les credentials vont générer des variables d'environnement (ou de simple variables) qui pourront par la suite être utilisés par les objets qui en ont besoin.

Il est même possible de créer ses propres type de credentials personnalisés pour l'utilisation d'un module Ansible par exemple.

## Execution Environment (Environnement d'execution)

Un execution environment est un container préconfiguré pour le lancement des playbooks.
Il peut contenir des collections, des binaires, des module python préinstallés ou des modifications personnalisées comme l'installation de certificats d'entreprise par exemple.

## Job Templates (Modèle de travail)

Les Job templates vont servir de modèle pour le lancement d'un playbook sur un inventaire de machine donné.
Il fait le lien entre les objets suivants :
- Project -> Selectionne un playbook
- Inventory
- Credentials
- Execution Environment

Un job basé sur ce modèle est par la suite créé pour lancer le playbook. 

# Fonctionnement

![AWX process](AWX_Process.svg)

Lorsque le lancement d'un job est demandé, AWX va par défault synchroniser ses sources en premier afin que celles-ci soient à jour.
Ensuite, le job se lance à sont tour en utilisant les objets défini par sont Job Template.