---
layout: post
title: Passer de Wordpress à Jekyll
tag:
featured-img: wp2jekyll
---
Il y avait longtemps que j'avais envie d'essayer [Jekyll](https://jekyllrb.com/){:target="_blank" }. C'est fait ! J'ai migré ce blog de [Wordpress](https://fr.wordpress.org/){:target="_blank"} vers [Jekyll](https://jekyllrb.com/){:target="_blank"} (vous pouvez, pendant encore quelque temps, consulter [la version Wordpress de ce blog](http://liltools.lacherez.info/), qui naturellement ne sera plus mise à jour).

Je consigne ici quelques notes sur la démarche que j'ai utilisée pour la migration.

# Récupération des billets du blog
Il existe des [plugins pour importer dans Jekyll des contenus provenant de différents CMS et plateformes de blog](XXX Lien). Naturellement, il en existe un pour Wordpress, mais 


# Récupération des images du blog

# Images de couverture

```bash
 $ ls|sed 's/\(.*\)\.\(.*\)/convert \1.\2 -resize 535px \1.jpg/'
 $ ls *.*|sed 's!\(.*\)\.\(.*\)!convert \1.\2 images/\1.jpg!'
 ```
TODO : ajouter un .htaccess pour ne pas perdre les liens
