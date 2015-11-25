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

### Création d'une application

* créer l'application `edition`

        $ python manage.py startapp edition

* survol fichiers
* coder des modèles<br />
`edition/models.py`
    * documentation :
        * [https://docs.djangoproject.com/en/1.8/topics/db/models/](https://docs.djangoproject.com/en/1.8/topics/db/models/)
        * [https://docs.djangoproject.com/en/1.8/ref/models/fields/](https://docs.djangoproject.com/en/1.8/ref/models/fields/)
    * yeah! on code!
        * Revue.nom
        * Auteur.nom

              from django.db import models

              class Revue(models.Model):
                  pass  # code fields here

              class Auteur(models.Model):
                  pass  # code fields here

* installer l'application `edition`<br />
`erudit/settings.py`

        INSTALLED_APPS = (
            # ...
            'edition',
        )


### Commandes Django

        $ python manage.py help

### Migration

* créer une migration

        $ python manage.py makemigrations

* appliquer des migrations

        $ python manage.py migrate

### Backend : gérer les données dans l'admin

* l'admin est installé par défaut avec Django 1.8
    * `settings.py`
    * `urls.py`

* les tables nécessaires à l'application admin sont créées lors de la première migration

      $ python manage.py migrate

* [http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/)

* créer un utilisateur superadmin

      $ python manage.py createsuperuser

* enregistrer les modèles dans l'admin<br />
`edition/admin.py`

        from django.contrib import admin

        from edition.models import Revue, Auteur


        admin.site.register(Revue)
        admin.site.register(Auteur)

* améliorer les modèles pour une meilleure lisibilité dans l'admin<br />
`edition/models.py`

        def __str__(self):
            return ""  # coder ici la chaîne de caractères désirée que l'on veut retourner

* ajouter de façon itérative plus d'infos sur nos modèles
    * Editeur
    * Numero
    * Article
    * Auteur.prenom
    * Auteur.date_naissance

* créer et appliquer la migration :

        $ python manage.py makemigrations

        $ python manage.py migrate

* ajouter une relation entre `Article` et `Auteur`<br />
    `edition/models.py`
    * ajouter :<br />
    `Article.auteurs`
    * modifier la base de données en conséquence (migration)

* ajouter quelque données dans l'admin

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

              from django.shortcuts import render

              def home(request):
                  c = { }
                  return render(request, 'home.html', c)

* `templates` [https://docs.djangoproject.com/en/1.8/topics/templates/](https://docs.djangoproject.com/en/1.8/topics/templates/)
    * créer répertoire `templates` dans `erudit`
    * par défaut Django chercher les templates dans les répertoires `templates` des applications installées
    * configurer le projet pour `erudit` soit une app installée<br />
    `erudit/settings.py`

              INSTALLED_APPS = (
                  [...]
                  'erudit',
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

        from edition.models import *
        articles = Article.objects.all()
        for a in articles: print(a)
        a = articles[0]
        a.auteurs.all()
        au = Auteur.objects.get(id=1)
        au.id
        au = Joueur.objects.get(id=314)
        auteurs = Auteur.objects.filter(article__nom__startswith='Laozi')

* créer une relation entre Revue et Editeur

* créer des données dans l'admin

* tester l'accès à l'Editeur via une Revue, et vice versa

        revue = Revue.objects.get(id=1)
        revue.editeur
        e_id = rvue.editeur.id
        editeur = editeur.objects.get(id=e_id)
        editeur.revues
        editeur.revue_set.all()

* faciliter la syntaxe d'accès aux objets reliés<br />
`edition/models.py`

        related_name = "revues"

* relancer shell

        editeur.revues.all()
        editeur.revues.count()

* passer les variables pertinentes pour accueil<br />
suite à exploration interactive via l'ORM dans le shell<br />
`erudit/views.py`

* boucler sur des querysets dans le template<br />
`erudit/templates/home.html`

        {% for r in revues %}
        {% endfor %}

---

## PARTIE 3 : PROJET ERUDIT AMÉLIORÉ

### Héritage de templates

* `erudit/templates/base.html`

        {% block main %}
        {% endblock %}

* `templates/`

        {% extends "base.html" %}

        {% block main %}
        {% endblock %}

### URL avec paramètres

* import des urls d'une app, ici celle de l'app `edition`<br />
`erudit/urls.py`

        urlpatterns = patterns('',
            # ...
            url(r'^', include('edition.urls')),
        )

* pages de détail : capter l'id de l'objet dans l'URL<br />
`edition/urls.py`

        from django.conf.urls.defaults import patterns

        urlpatterns = patterns('edition.views',
            url(r'^revues/(?P<id>\d+)$', 'revue_detail', name="revue"),
        )

* capter l'id de l'objet dans la vue et aller chercher l'objet en DB<br />
`edition/views.py`

        from django.shortcuts import render

        from edition.models import Revue

        def revue_detail(request, id):
            revue = Revue.objects.get(id=id)
            c = {
                'revue': revue,
            }
            return render(request, "edition/revue_detail.html", c)

* utiliser l'objet dans le template<br />
`edition/templates/edition/revue_detail.html`

        {{ revue.nom }}

### Admin amélioré : ModelAdmin

[https://docs.djangoproject.com/en/1.8/ref/contrib/admin/](https://docs.djangoproject.com/en/1.8/ref/contrib/admin/)

* classes héritant de `ModelAdmin` : `AuteurAdmin`, `ArticleAdmin`<br />
`edition/admin.py`

        class ArticleAdmin(admin.ModelAdmin):
            pass

        class AuteurAdmin(admin.ModelAdmin):
            pass

* enregistrer Modele avec ModeleAdmin

        admin.site.register(Article, ArticleAdmin)
        admin.site.register(Auteur, AuteurAdmin)

* configuration des ModeleAdmin

        list_display
        search_fields
        list_filter

* plus? [https://docs.djangoproject.com/en/1.8/ref/contrib/admin/](https://docs.djangoproject.com/en/1.8/ref/contrib/admin/)
    * list_display_link
    * list_editable
    * readonly_fields
    * inlines
    * fieldsets

### Saut dans le temps : télécharger les sources

* Télécharger les [sources du projet amélioré][projet-pimped]

* Extraire

* Amélioré? Quoi de neuf?

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

### Templates améliorés

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

## PARTIE 4 : HANDS-ON : CRÉER L'APPLICATION ABONNEMENT POUR LE PROJET ERUDIT

### Créer une application abonnement

### Ajouter les modèles Bibliotheque et Abonnement

* Biblioheque
    * nom
* Abonnement
    * bibliotheque
    * revue
    * annee

### Autre exercice : âge d'un auteur

* Auteur.date_naissance
* Auteur.age()<br />
*ajouter une methode `age()` sur la classe Auteur utilisant la date de naissance*

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
[projet-pimped]: http://bit.ly/1g3mfj4
