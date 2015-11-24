# Ateliers Django - Setup technique

---

## Installer Python Anaconda

Installez la distribution Python "Anaconda", de Continuum analytics, en suivant les instructions sur leur site : https://www.continuum.io/downloads

* choisir la version de Python appropriée pour l'atelier
  * 2.7
  * 3.5
* attention, téléchargement d'environ 300 Mo
* si à l'installation Anaconda vous demande si vous voulez qu'il soit votre Python par défaut, dites "oui", ex.:

        """
        Do you wish the installer to prepend the Anaconda3 install location
        to PATH in your /home/giotta/.bashrc ? [yes|no]
        """

## Installer Django

* dans le terminal, tapez

        $ conda install django

* fermez le terminal utilisé pour l'installation

## Tester l'installation

1. lancez un nouveau terminal
1. tapez "ipython" + enter

        $ ipython

1. vous devriez voir un prompt IPython qui annonce la version de Python, ex :

        Python 3.5.0 |Anaconda 2.4.0 (64-bit)| (default, Oct 19 2015, 21:57:25)
        [...]
        In [1]:

1. importez django et testez sa version

        In [1]: import django

        In [2]: django.get_version()
        Out[2]: '1.8.4'

1. fermez IPython en tapant "exit"
