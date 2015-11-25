# Atelier Django : Introduction de base

---

## INTRODUCTION

* Objectifs
    * connaître l'**architecture d'un projet** : principaux fichiers
    * gérer data dans **backend** *(admin django)*
    * présenter data dans **frontend**
* Documentation
    * doc : choisir la bonne version de django
        * [officielle][django-doc] *(en anglais)*
        * [français][django-doc-fr]
    * tutoriel
    * [paquets][django-packages]
* Environnement
    * éditeur texte : gedit, vi/vim, emacs, nano, atom...
    * interpréteur Python : python, ipython
* [Setup technique][setup]

---

## PARTIE 1 : DÉVELOPPEMENT WEB AVEC DJANGO : ARCHITECTURE D'UN PROJET

### Développement web

* schéma : le [développement web][schema-web-development]
    * rôle d'un framework web : aider à construire réponse à une requête
* environnements
    * développement (DEV)
    * déploiement (TEST, PROD)

### Django

Pourquoi Django vs autre framework (ex.: CherryPy, Pyramid, web2py)

* tout-en-un
* admin : meilleur vendeur ;)
* docs
* communauté : utilisation répandue

Principaux fichiers

* `urls.py`
* `views.py`
* `models.py`
* `templates` (HTML)
* `admin.py`

### Projet : définition du besoin

> Érudit fait la refonte de sa plateforme web.<br />
On vous contacte pour créer un site web qui pourrait gérer les articles de revues produits par les éditeurs...<br />
... et gérer les abonnements des bibliothèques.

### Projet : modélisation

* Editeur
* Revue
* Numero
* Article
* Auteur
* Bibliotheque
* Abonnement

---

## PARTIE 2 : HANDS-ON : PROJET ET APPLICATIONS

### Création du projet

* créer un répertoire `atelier`

        ~$ mkdir atelier
        ~$ cd atelier

* créer le projet `erudit`

        $ django-admin.py startproject erudit

* se déplacer dans le projet django créé

        $ cd erudit

* survol des fichiers générés
* lancer le serveur de développement

        $ python manage.py runserver

* configurer la base de données<br />
`erudit/settings.py`

        DATABASES = {
            'default': {
                'ENGINE': 'django.db.backends.sqlite3', # Add 'postgresql_psycopg2', 'mysql', 'sqlite3' or 'oracle'.
                'NAME': 'erudit.db',                      # Or path to database file if using sqlite3.
                'USER': '',                      # Not used with sqlite3.
                'PASSWORD': '',                  # Not used with sqlite3.
                'HOST': '',                      # Set to empty string for localhost. Not used with sqlite3.
                'PORT': '',                      # Set to empty string for default. Not used with sqlite3.
            }
        }

### Création d'une application

* créer l'application `ligue`

        $ python manage.py startapp ligue

* survol fichiers
* coder des modèles<br />
`ligue/models.py`
    * documentation :
        * [https://docs.djangoproject.com/en/1.8/topics/db/models/](https://docs.djangoproject.com/en/1.8/topics/db/models/)
        * [https://docs.djangoproject.com/en/1.8/ref/models/fields/](https://docs.djangoproject.com/en/1.8/ref/models/fields/)
    * yeah! on code!
        * Equipe.nom
        * Joueur.nom

              from django.db import models

              class Equipe(models.Model):
                  pass  # code fields here

              class Joueur(models.Model):
                  pass  # code fields here

* installer l'application `ligue`<br />
`erudit/settings.py`

        INSTALLED_APPS = (
            # ...
            'ligue',
        )

### South

* installer South dans le projet<br />
`erudit/settings.py`

        INSTALLED_APPS = (
            # ...
            'south',
        )

* créer une migration pour une nouvelle application

        $ python manage.py schemamigration ligue --initial

* *créer une migration pour un application existante (pas le cas ici)*

        $ python manage.py schemamigration --auto

* créer les tables de south

        $ python manage.py syncdb

* appliquer les migrations

        $ python manage.py migrate ligue

### Backend : gérer les données dans l'admin

* activer les urls permettant l'accès à l'interface d'admin : *décommenter*<br />
`erudit/urls.py`

        # ...

        from django.contrib import admin
        admin.autodiscover()

        urlpatterns = patterns('',
            # ...
            url(r'^admin/', include(admin.site.urls)),
        )

* [http://127.0.0.1:8000/](http://127.0.0.1:8000/)
* installer l'application d'admin dans le projet<br />
`erudit/settings.py`

        INSTALLED_APPS = (
            # ...
            'django.contrib.admin',
            # ...
        )

* créer les tables de l'admin

      $ python manage.py syncdb

* [http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/)

* enregistrer les modèles dans l'admin : *[voir tutoriel Part 2](https://docs.djangoproject.com/en/1.8/intro/tutorial02/)*<br />
`ligue/admin.py`

        from django.contrib import admin

        from ligue.models import Equipe, Joueur

        admin.site.register(Equipe)
        admin.site.register(Joueur)

* pimper les modèles pour une meilleure lisibilité dans l'admin<br />
`ligue/models.py`

        def __unicode__(self):
            return ""  # coder ici la chaîne unicode désirée que l'on veut retourner

* ajouter de façon itérative plus d'infos sur nos modèles
    * Joueur.prenom
    * Joueur.date_naissance

* Si south est installé, créer et appliquer la migration :

        $ python manage.py schemamigration ligue --auto

        $ python manage.py migrate ligue

* Sinon :
    * supprimer `erudit.db`
    * recréer les tables

          $ python manage.py syncdb

* ajouter une relation entre `Joueur` et `Equipe`<br />
    `ligue/models.py`
    * ajouter :<br />
    `Joueur.equipe`
    * modifier la base de données en conséquence :
        * south : créer et appliquer la migration
        * autre : supprimer la DB et syncdb

* ajouter quelque Equipes et Joueurs dans l'admin

### Frontend : présenter les données

* créer une URL pour la page d'accueil : [https://docs.djangoproject.com/en/1.8/topics/http/urls/](https://docs.djangoproject.com/en/1.8/topics/http/urls/)<br />
`erudit/urls.py`

        urlpatterns = patterns('',
            url(r'^$', 'erudit.views.home', name='home'),
            # ...
        )

* créer le code qui va générer la page d'accueil<br />
`erudit/views.py`
    * [https://docs.djangoproject.com/en/1.8/topics/http/views/](https://docs.djangoproject.com/en/1.8/topics/http/views/)
    * *[voir tutoriel Part 3 (version shortcut)](https://docs.djangoproject.com/en/1.8/intro/tutorial03/)*

              from django.shortcuts import render

              def home(request):
                  c = { }
                  return render(request, 'home.html', c)

* `templates` [https://docs.djangoproject.com/en/1.8/topics/templates/](https://docs.djangoproject.com/en/1.8/topics/templates/)
    * créer répertoire `templates` dans `erudit`
    * configurer le projet pour qu'il sache où aller chercher par défaut les templates<br />
    `erudit/settings.py`

              import os

              # ...

              TEMPLATE_DIRS = (
                  os.path.join(os.path.dirname(__file__), "templates"),
              )

* créer le template de la page d'accueil<br />
`erudit/templates/home.html`

* passer une variable au template<br />
`erudit/views.py`

* utiliser une variable dans les templates<br />
`erudit/templates/home.html`

        {{ var }}

* `views.py` : *endroit où on code la logique en Python...<br />
...plus simple si explore interactivement.*

        $ python manage.py shell

* ORM (API) : object relation mapping

        from ligue.models import *
        equipes = Equipe.objects.all()
        for e in equipes: print e
        e = equipes[0]
        e.joueur_set.all()
        j = Joueur.objects.get(id=1)
        j.id
        j = Joueur.objects.get(id=314)
        e.id
        joueurs = Joueur.objects.filter(equipe__nom__startswith='Pyth')

* faciliter la syntaxe d'accès aux objets reliés<br />
`ligue/models.py`

        related_name = "joueurs"

* relancer shell

        e.joueurs.all()
        e.joueurs.count()

* passer les variables pertinentes pour accueil<br />
suite à exploration interactive via l'ORM dans le shell<br />
`erudit/views.py`

* boucler sur des querysets dans le template<br />
`erudit/templates/home.html`

        {% for e in equipes %}
        {% endfor %}

---

## PARTIE 3 : PROJET LRMP PIMPÉ

### Héritage de templates

* `erudit/templates/base.html`

        {% block main %}
        {% endblock %}

* `templates/`

        {% extends "base.html" %}

        {% block main %}
        {% endblock %}

### URL avec paramètres

* import des urls d'une app, ici celle de l'app `ligue`<br />
`erudit/urls.py`

        urlpatterns = patterns('',
            # ...
            url(r'^', include('ligue.urls')),
        )

* pages de détail : capter l'id de l'objet dans l'URL<br />
`ligue/urls.py`

        from django.conf.urls.defaults import patterns

        urlpatterns = patterns('ligue.views',
            url(r'^equipes/(?P<id>\d+)$', 'equipe_detail', name="equipe"),
        )

* capter l'id de l'objet dans la vue et aller chercher l'objet en DB<br />
`ligue/views.py`

        from django.shortcuts import render

        from ligue.models import Equipe

        def equipe_detail(request, id):
            equipe = Equipe.objects.get(id=id)
            c = {
                'equipe': equipe,
            }
            return render(request, "ligue/equipe_detail.html", c)

* utiliser l'objet dans le template<br />
`ligue/templates/ligue/equipe_detail.html`

        {{ equipe.nom }}

### Admin pimpé : ModelAdmin

[https://docs.djangoproject.com/en/1.8/ref/contrib/admin/](https://docs.djangoproject.com/en/1.8/ref/contrib/admin/)

* classes héritant de `ModelAdmin` : `JoueurAdmin`, `EquipeAdmin`<br />
`ligue/admin.py`

        class EquipeAdmin(admin.ModelAdmin):
            pass

        class JoueurAdmin(admin.ModelAdmin):
            pass

* enregistrer Modele avec ModeleAdmin

        admin.site.register(Equipe, EquipeAdmin)
        admin.site.register(Joueur, JoueurAdmin)

* configuration des ModeleAdmin

        list_display
        search_fields
        list_filter

* plus? [https://docs.djangoproject.com/en/1.8/ref/contrib/admin/](https://docs.djangoproject.com/en/1.8/ref/contrib/admin/)
    * fields
    * fieldsets
    * ...

### Saut dans le temps : télécharger les sources

* Télécharger les [sources du projet pimpé](http://bit.ly/1g3mfj4)

* Extraire

* Pimpé? Quoi de neuf?

### Fichiers statiques : CSS, images et js

[https://docs.djangoproject.com/en/1.8/howto/static-files/](https://docs.djangoproject.com/en/1.8/howto/static-files/)

* répertoire : `erudit/static`
    * `css`
    * `images`
    * `js`

* `erudit/settings.py`

        PROJECT_ROOT = os.path.dirname(__file__)
        SITE_ROOT = os.path.dirname(PROJECT_ROOT)

        MEDIA_ROOT = os.path.join(PROJECT_ROOT, 'media')
        MEDIA_URL = '/media/'

        STATIC_ROOT = os.path.join(SITE_ROOT, 'site_static')
        STATIC_URL = '/static/'
        STATICFILES_DIRS = (
            os.path.join(PROJECT_ROOT, 'static'),
        )

* `erudit/urls.py`

        from django.contrib.staticfiles.urls import staticfiles_urlpatterns

        # ...

        urlpatterns += staticfiles_urlpatterns()

* `templates`

        {{ STATIC_URL }}

### Templates pimpés

* `erudit/templates/base.html`

### Connexion du user

* `erudit/templates`
    * `connexion.html`
    * `deconnexion.html`

* `erudit/urls.py`

        urlpatterns = patterns('',
            # ...
            url(r'^login/$', 'django.contrib.auth.views.login',
                dict(template_name='connexion.html',),
                'connexion'
            ),
            url(r'^logout/$', 'django.contrib.auth.views.logout',
                dict(template_name='deconnexion.html',),
                'deconnexion'
            ),
            # ...
        )

* `erudit/settings.py`

        LOGIN_URL = "/login/"
        LOGIN_REDIRECT_URL = "/"

---

## PARTIE 4 : HANDS-ON : CRÉER L'APPLICATION CALENDRIER POUR LE PROJET LRMP

### Créer une application calendrier

### Ajouter les modèles Match et Saison

* Match
    * date
    * lieu
    * equipe1
    * equipe2
    * score1
    * score2
* Saison
    * ...

### Autre exercice : âge d'un joueur

* Joueur.date_naissance
* Joueur.age()<br />
*ajouter une methode `age()` sur la classe Joueur utilisant la date de naissance*

---

## CONCLUSION : POUR CONTINUER

* Autres aspects non couverts (quelques uns)
    * permissions et décorateurs
    * manage.py inspectdb *[legacy database](https://docs.djangoproject.com/en/1.8/howto/legacy-databases/)*
    * fixtures : [données initiales et de test](https://docs.djangoproject.com/en/1.8/howto/initial-data/)
    * manage.py test
    * forms
    * generic views
    * templates : tags, filtres

* Mailing list des utilisateurs Django :<br />
[http://groups.google.com/group/django-users](http://groups.google.com/group/django-users)

* Channel IRC #django :<br />
irc://irc.freenode.net/django

* Contribs, plugins<br />
[https://docs.djangoproject.com/en/dev/ref/contrib/](https://docs.djangoproject.com/en/dev/ref/contrib/)
   * south
   * reversion
   * Autre : pypi<br />
[http://pypi.python.org/](http://pypi.python.org/)

* [Montréal-Python][mtlpy] :
    * entrer dans la communauté
    * événements à venir

* Enjoy!

---

[django-doc]: https://docs.djangoproject.com/en/1.8/
[django-doc-fr]: https://docs.djangoproject.com/fr/1.8/
[django-packages]: https://www.djangopackages.com/
[schema-web-development]: ./web-development.jpg
[setup]: ../setup.md
[mtlpy]: http://montrealpython.org/
