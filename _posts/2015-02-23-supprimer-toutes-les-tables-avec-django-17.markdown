---
layout: post
title: "Supprimer toutes les tables avec Django 1.7+"
date: 2015-02-23 17:07:34 +0100
comments: true
categories: Django
---

Lorsque l'on veut repartir d'une base vide avec un projet Django 1.7+ qui utilise les migrations, on peut utiliser la commande:

```shell
# Remplacer <app> par le nom de votre application
./manage.py migrate <app> zero
```

La migration `zero` correspond à l'état juste avant que la première migration ait été appliquée. Cette commande va ainsi annuler toutes les migrations et supprimer toutes les tables.

C'est particulièrement utile lorsque l'on travaille avec une base PostgreSQL puisque cela permet d'éviter de supprimer manuellement la base et de la recréer.
