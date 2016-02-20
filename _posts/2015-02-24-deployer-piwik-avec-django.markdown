---
layout: post
title: Déployer Piwik avec Django
tags:
- Django
---

Aujourd'hui, j'ai voulu intégrer [Piwik Web Analytics](http://piwik.org/) dans une application Django sans ajouter de nouvelle dépendance. La solution que j'ai mise en place s'appuie sur le fichier de settings et la création d'un [context processor](https://docs.djangoproject.com/en/dev/ref/templates/api/#writing-your-own-context-processors).

## Définition des variables Piwik dans `settings.py`

Dans le fichier `settings.py`, ajouter des variables pour l'URL de votre instance Piwik et l'ID du site.

```python
PIWIK_IDSITE = '123'
PIWIK_URL = 'analytics.example.com'
```

## Création du context processor

Créer un fichier `context_processor.py` qui lit les variables de Piwik dans le fichier de settings et les rends accessibles aux templates.

```python
from django.conf import settings


def piwik_analytics(request):
    return {
        'PIWIK_URL': settings.PIWIK_URL,
        'PIWIK_IDSITE': settings.PIWIK_IDSITE,
    }
```

## Ajout du context processor au settings

Dans le fichier `settings.py`, ajouter le context processor à `TEMPLATE_CONTEXT_PROCESSORS`. Le plus simple pour conserver les context processors par défaut de Django est de les importer depuis `global_settings`.

```python
from django.conf import global_settings


TEMPLATE_CONTEXT_PROCESSORS = global_settings.TEMPLATE_CONTEXT_PROCESSORS + (
    'website.context_processors.piwik_analytics',
)
```

## Créer un template pour Piwik

Je préfère créer un fichier de template spécifique `piwik.html` qui contient le [JavaScript de tracking](http://developer.piwik.org/guides/tracking-javascript-guide) du site. Ce template peut référencer les variables `PIWIK_URL` et `PIWIK_IDSITE` définies dans le context processor.

{% raw %}
```erb
{% if PIWIK_IDSITE %}
<script type="text/javascript">
    var _paq = _paq || [];
    (function(){ var u=(("https:" == document.location.protocol) ? "https://{{ PIWIK_URL }}/" : "http://{{ PIWIK_URL }}/");
    _paq.push(['setSiteId', {{ PIWIK_IDSITE }}]);
    _paq.push(['setTrackerUrl', u+'piwik.php']);
    _paq.push(['trackPageView']);
    _paq.push(['enableLinkTracking']);
    var d=document, g=d.createElement('script'), s=d.getElementsByTagName('script')[0]; g.type='text/javascript'; g.defer=true; g.async=true; g.src=u+'piwik.js';
    s.parentNode.insertBefore(g,s); })();
</script>
{% endif %}
```
{% endraw %}

Ce script est inclus uniquement quand on a renseigné le `PIWIK_IDSITE`. Ceci permet de choisir de ne pas envoyer de statistiques lorsqu'on est en développement par exemple.

## Intégrer le template Piwik dans le site

La [documentation de Piwik](http://developer.piwik.org/guides/tracking-javascript-guide) conseille d'ajouter le tracking code juste après le tag `<body>`. Dans votre fichier `base.html` ou équivalent, il faut donc inclure le template piwik que l'on vient de créer.

{% raw %}
```erb
(...)
</head>
<body>
{% include 'piwik.html' %}
(...)
```
{% endraw %}
