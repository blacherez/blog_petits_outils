---
layout: post
title: Gérer les dépendances d'un projet Python avec Anaconda et Pip
tag: python, pip, anaconda
featured-img: parcel
---
Le gestionnaire de modules [pip](https://pypi.org/project/pip/) permet d'installer tous les modules nécessaires à un projet Python en utilisant un fichier `requirements.txt` à la racine du projet, dans lequel les modules requis sont listés, avec la version nécessaire :

```bash
$ pip install -r requirements.txt
```

De même, il est facile de générer ce fichier :

```bash
$ pip freeze > requirements.txt
```

J'ai l'habitude d'utiliser [Anaconda](https://www.anaconda.com/what-is-anaconda/) pour gérer différents environnements Python. Toutefois, quand je commence un projet, j'aime avoir des outils pratiques comme `ipython` pour expérimenter et j'essaie souvent différentes solutions avant d'arriver à celle qui me donne satisfaction. Cela m'amène à ajouter beaucoup de modules variés dans mon environnement, dont beaucoup ne sont finalement pas nécessaires dans le produit final. `pip freeze` renvoie alors une liste de modules beaucoup trop longue qu'il est fastidieux de corriger. De même, il est fastidieux pendant les premiers essais de tenir une liste à jour des modules que j'installe : il arrive que j'en teste un, puis un autre, avant de revenir au premier. J'ai donc adopté une solution simple :

- Je commence mon projet avec mon environnement habituel, que j'ai appelé `py3`, dans lequel tous les modules qui me sont souvent nécessaires sont installés (`pandas`, `ipython`, `jupyter`, `BeautifulSoup` etc.) :

```bash
$ source ~/anaconda3/bin/activate py3
```

- Si j'ai besoin d'un nouveau module, je l'ajoute avec `pip`.

```bash
(py3) $ pip install XXX
```

Par ailleurs, j'ai créé, une fois pour toutes, un environnement `py3-minimal`, avec python 3 :

```bash
$ ~/anaconda3/bin/conda create -n py3-minimal python=3
```

Quand mon projet est suffisamment avancé, je crée un nouvel environnement spécifique au projet, en copiant mon environnement minimal, puis j'active ce nouvel environnement (la [conda cheat sheet](https://conda.io/docs/_downloads/conda-cheatsheet.pdf) est utile pour se souvenir de la syntaxe des commandes `conda`) :

```bash
(py3) $ source ~/anaconda3/bin/deactivate
$ ~/anaconda3/bin/conda create --clone py3-minimal -n mon_projet
$ source ~/anaconda3/bin/activate mon_projet
```

Il ne me reste alors qu'à installer les différents modules nécessaires avec `pip`. Si j'en oublie, j'obtiens une erreur en essayant de tester le programme et il me suffit de les ajouter. Je peux alors faire mon `pip freeze > requirements.txt` sans avoir d'éléments inutiles.
