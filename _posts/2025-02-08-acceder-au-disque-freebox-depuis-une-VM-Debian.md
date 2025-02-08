---
layout: post
title: Accéder au disque dur de la Freebox depuis une VM Debian
tags:
- sysadmin
- freebox
---

## Configurer le partage sur la Freebox

Pour accéder au disque de la Freebox depuis une VM, il est nécessaire d'en
premier lieu activer les paramètres suivants sur la Freebox. Pour cela il faut
accéder à <mafreebox.freebox.fr> puis après s'être authentifié :

- Ouvrir « Paramètres de la Freebox »
- Mode avancé
- Partage de fichiers | Partages Windows
- Activer SMB2/SMB3 : coché
- Activer le partage de fichiers : coché
- Accès authentifié : décoché

## Configurer le partage sur la VM

Se connecter sur la VM puis installer le paque `cifs-utils` :

```sh
sudo apt install cifs-utils
```

Préparer le point de montage :

```sh
sudo mkdir /media/freebox
```

Éditer le fichier `/etc/fstab` et ajouter le point de montage vers votre
disque. Dans mon cas le disque s'appelle `RAID Freebox`, il est nécessaire
d'encoder l'espace pour qu'il soit correctement pris en compte par fstab :

```text
//mafreebox.freebox.fr/RAID\040Freebox /media/freebox cifs guest,uid=1000,gid=1000 0 0
```

Il peut être nécessaire de recharge la configuration pour `systemd` avant de
relancer le montage :

```sh
sudo systemctl daemon-reload
sudo mount -a
```

Vos fichiers sont maintenant accessibles dans `/media/freebox` et le répertoire
sera monté automatiquement à chaque démarrage de la Freebox.
