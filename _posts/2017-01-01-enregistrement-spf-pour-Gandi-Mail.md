---
layout: post
title: Enregistrement SPF pour Gandi Mail
tags:
- sysadmin
---

Ayant un nom de domaine chez Gandi, j'utilise aussi Gandi Mail pour mes
adresses associées à ce nom de domaine.

Malheureusement, la configuration par défaut de Gandi Mail ne contient pas
d'enregistrement SPF. Or, si votre domaine n'a pas d'enregistrement SPF,
certains domaines destinataires risquent de rejeter les messages car ils ne
pourront pas les authentifier comme provenant d'un serveur de messagerie
autorisé. C'est le cas par exemple avec Gmail qui envoie dans le dossier Spam
les messages reçus depuis le domaine.

Pour mettre en place un enregistrement SPF sur un nom de domaine Gandi, il faut
[modifier la configuration de la zone du
domaine](https://wiki.gandi.net/fr/dns/zone/spf-record) et ajouter l'entrée suivante :

```
@ 10800 IN TXT "v=spf1 include:_mailcust.gandi.net ?all"
```

Après validation de cette nouvelle configuration et une fois la propagation DNS terminée, les messages ne sont
plus considérés comme des spams.
