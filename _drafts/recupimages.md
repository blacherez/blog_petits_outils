---
layout: post
title: Télécharger les images d'un site web
tag:
featured-img: photos
---
J'ai eu besoin récemment à plusieurs reprises de télécharger le contenu d'un site web ([par exemple quand j'ai migré ce blog de Wordpress vers Jekyll]({% link _posts/2018-05-15-wp2jekyll.md %})). J'avais à chaque fois un moyen simple de récupérer le texte du site, mais j'ai dû télécharger les images à part. C'est pour cela que j'ai écrit le script `recupimages.py`.

[Retrouvez ce script sur Github](https://github.com/blacherez/recupimages)

# Fonctionnement général

Les premières lignes du script permettent de définir la configuration :

```python
## Configuration, à adapter
EXTENSIONS = [".png", ".jpg", ".jpeg", ".gif"] # Extensions autorisées pour nos images
BACKUP_DIR = "backup" # Répertoire dans lequel on copie les fichiers d'origine avant de les modifier
BASE_URL = "http://outils.lacherez.info" # URL de base du site d'origine pour compléter les URL relatives
IMAGE_DIR = "archives_images" # Répertoire pour les images
IMAGE_PREFIX = "/assets/imgs/archives_images" # Chemin vers le répertoire pour les images pour les liens (chemin HTTP final)
## Fin de la configuration
```
Après avoir modifié ces valeurs, il suffit de lancer le script en lui donnant la liste des fichiers HTMLà modifier en arguments. La démarche détaillée est expliquée dans le fichier `README.md` du projet. Les fichiers HTML concernés seront lus, chaque image référencée sera téléchargée localement et le fichier HTML sera modifié pour tenir compte du nouvel emplacement. Naturellement, le nouvel emplacement dépendra de la façon dont le site sera installé sur le nouveau serveur (par exemple, il pourra être dans un sous-répertoire d'un autre site ou bien sur sa propre adresse), c'est pour s'adapter à ces différentes situations qu'un préfixe (`IMAGE_PREFIX`) doit être fourni. Par exemple, j'ai utilisé ce script pour les images d'un blog, le nom de mon répertoire local était `archives_images`, mais comme les images de mon sites sont situées dans un répertoire `assets/img`, c'est là que j'ai placé le répertoire que j'ai obtenu. Ainsi, les images sont accessibles à l'adresse `/assets/imgs/archives_images` et cette adresse qui devra être utilisée dans le HTML et qui devra donc apparaître dans `IMAGE_PREFIX`.

# Détails du Fonctionnement

L'essentiel du travail est fait dans la fonction `traite()` :

```python
def traite(contenu):
    images = liste_images(contenu, True)
    #print(images)
    correspondances = {}
    for i in images:
        correspondances[i] = get_image(i, IMAGE_DIR, IMAGE_PREFIX)
        #print(correspondances)
    if len(correspondances):
        pattern = re.compile('|'.join(correspondances.keys()))
        r = pattern.sub(lambda x: correspondances[x.group()][1], contenu)
        return r
    return contenu
```

Cette fonction prend du contenu HTML en paramètre (sous la forme d'une chaîne de caractères). Elle commence par lister les images avec la fonction `liste_images()`, celle-ci renvoie une liste de toutes les images référencées dans la page. Si une image sert d'ancre pour un lien et que ce lien pointe aussi vers une image, cette seconde image est ajoutée à la liste (ce comportement peut être modifié en passant `False` comme second paramètre à `liste_images()`).

Cette liste d'images est ensuite utilisée pour télécharger les images et construire un dictionnaire (`correspondances`) qui associe le nom de l'image tel qu'il apparaît dans le contenu HTML avec son nouveau chemin (sur le disque) et sa nouvelle adresse (HTTP). C'est ce que fait la fonction `get_image()`.

Ensuite, on crée une expression régulière constituée de toutes les anciennes adresses (`re.compile('|'.join(correspondances.keys()))`). Cette expression est ensuite utilisée pour faire des substitutions dans le contenu (`pattern.sub(lambda x: correspondances[x.group()][1], contenu)`) : chaque occurrence d'une adresse est remplacée par la nouvelle adresse.
