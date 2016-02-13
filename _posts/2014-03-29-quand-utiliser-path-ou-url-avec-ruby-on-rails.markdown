---
layout: post
title: "Quand utiliser les helpers _path et _url avec Ruby on Rails"
date: 2014-03-29 14:23:50 +0100
comments: true
published: true
categories: rails
---
Il y a deux façons d'exprimer les liens et URLs avec Ruby on Rails : `_path` -- qui exprime un chemin relatif -- et `_url` qui correspond au même chemin préfixé du nom d'hôte, du port et du path prefix.

D'après les [spécifications HTTP](http://www.ietf.org/rfc/rfc2616.txt), les redirections doivent spécifier une URI, ce qu'on peut interpréter comme un *fully qualified domain name* (nom de domaine complètement qualifié). On peut donc penser qu'il est préférable d'utiliser le helper `_url` lorsqu'on fait un `redirect_to` dans le code d'un controller.

Dans la pratique avec Ruby on Rails, on préfère utiliser `_path` plutôt que `_url`. En effet, le navigateur est capable de retrouver l'URL complète automatiquement en se basant sur les informations de la requête.

Pour résumer, si on veut être pointilleux il est conseillé d'utiliser `_url` pour les redirections et `_path` dans le code des vues (pour les liens par exemples). Quel que soit le choix effectué, le navigateur saura toujours reconstruire l'URL correctement.

Référence : [Obie Fernandez - Routing the Rails Way](https://www.informit.com/articles/article.aspx?p=1087656&seqNum=17)
