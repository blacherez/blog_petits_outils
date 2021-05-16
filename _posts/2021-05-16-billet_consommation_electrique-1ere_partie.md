---
layout: post
title: Quel CMS pour un site éco-responsable ? Partie 1
tag: green IT, éco-conception, web, CMS, wordpress, grav, website carbon, bilan carbone, consommation électrique, écologie, réchauffement climatique, éco-responsable
featured-img: notebook-405755_1920
---
# Quel CMS pour un site éco-responsable ? Première partie : le côté client

A la suite d'une récente conversation avec une amie, je me suis demandé comment il est possible de rendre un site web plus éco-responsable, et en particulier s'il est possible d'améliorer ses performances énergétiques en choisissant opportunément le CMS (content manager system, le logiciel qui gère le site, comme Wordpress, Drupal, Grav...) utilisé.

J'ai trouvé peu d'informations sur ce sujet particulier. Ce billet et le suivant (les suivants ?) sont destinés à faire une rapide synthèse de ces recherches et des petits tests que j'ai pu réaliser. J'ai en tout cas trouvé des informations intéressantes dans l'article de Cyrielle Willerval [Measure the server-side impact of your application with PowerAPI](https://blog.theodo.com/2020/05/greenit-measure-server-energy-consumption-powerapi/). En outre, j'ai enregistré un certain nombre de ressources sur [Pearltrees](https://www.pearltrees.com/blacherez/eco-conception-web/id41173117).


On peut considérer que deux dimensions sont à prendre en compte pour estimer la consommation d'énergie induite par un site web :

- Côté "client" : cette partie correspond aux besoins énergétiques pour transporter les données du site web depuis le serveur jusqu'au client (consommation des équipements intermédiaire, et consommation de l'appareil client), puis à afficher le site sur le client.
- Côté "serveur" : cette partie correspond aux besoins énergétiques pour générer les contenus web avant de les transmettre aux clients.

## Côté "client"

Cette partie concerne essentiellement le côté de l'utilisateur, ainsi que les équipements intermédiaires utilisés pour le transfert des données. La consommation d'énergie dépend de l'état final de la page HTML qui sera affichée par le navigateur client : taille de la page elle-même et des médias associés, complexité de la page, qui nécessitera plus ou de moins de calculs de la part du client (arborescence HTML, appelée DOM, en particulier) etc.

Cette dimension est importante, puisqu'elle croît forcément linéairement avec le nombre de consultations du site web : à peu de choses près, à chaque fois qu'une personne consulte un site, le coût énergétique s'applique. En outre, puisque cette consommation d'énergie sera pour l'essentiel à la charge de l'utilisateur, il nous saura gré, n'en doutons pas, de ne pas épuiser la batterie de sa tablette ou de son téléphone à chaque connexion sur notre site.

Elle est d'ailleurs plutôt bien documentée, avec des outils d'évaluation et des préconisations.

### Outils

Plusieurs outils peuvent être utilisés pour estimer ce coût côté client.

#### Website carbon

L'application en ligne website carbon donne quelques éléments sur l'empreinte carbone d'une page web, côté client. La lecture est simple et permet de donner des ordres de grandeur (classement par rapport aux autres sites testés, correspondances pour la masse de carbone produite etc.)

![Test de ce blog avec Website carbon](/asset/websitecarbon_outils.png)

#### GreenIT Analysis

Cet outil est une extension pour Chrome ou Firefox qui permet d'analyser une page affichée dans le navigateur (onglet ajouté aux "Outils de développement").

L'avantage de cet outil est qu'il peut être appliqué à n'importe quelle page affichée (donc même locale ou sur un réseau non public). En outre, il fournit des éléments sur la conformité de la page par rapport à des "bonnes pratiques".

![Test de ce blog avec GreenIT Analysis](/asset/greenit_outils.png)

#### Autres outils

Un site [ecoindex.fr](http://www.ecoindex.fr/) existe pour tester les sites web, mais il n'est pas accessible actuellement.

Un autre plugin,

#### Manuels et référentiels

Il existe plusieurs ressources sur l'écoconception de sites web, par exemple :

- Livre : [Ecoconception web : les 115 bonnes pratiques](https://ecoconceptionweb.com/)
- Le livre ci-dessus a été produit par un collectif qui fournit d'autres documents et même une certification : https://collectif.greenit.fr/ecoconception-web/
- [Comment être écolo et avoir un site web](https://constantin-boulanger.fr/comment-etre-ecolo-et-avoir-un-site-web/)
- [Comment mesurer l'empreinte carbone d'un site web ?](https://www.adimeo.com/blog/comment-mesurer-l-empreinte-carbone-d-un-site-web)


### Remarques sur le côté client

Grâce aux outils évoqués précédemment, l'aspect "client" de la consommation d'énergie est facile à tester. Il est toutefois assez difficile d'agir sur cet aspect : la plupart des utilisateurs, dont je suis, utilisent des CMS qui génèrent eux-mêmes le code HTML et ils ne créent pas les templates eux-mêmes. La seule marge qui est donnée repose sur quelques points relativement marginaux : nombre et paramétrage des médias (par exemple adapter la résolution d'une image à son utilisation pour éviter le transfert d'un gros fichier et son redimensionnement sur le client) essentiellement.

**Précaution concernant les outils évoqués** : il semble que les résultats donnés par Website carbon et GreenIT Analysis ne convergent pas toujours, sans doute parce que leurs modèles ne priorisent pas les défauts constatés de la même façon (voir captures d'écran ci-dessous où le même site, [lacherez.info](https://lacherez.info), est évalué comme très mauvais par Websitecarbon, avec 2,43 g de CO<sub>2</sub>, alors qu'il obtient un score de A avec GreenIT Analysis, avec 1,26 g de CO<sub>2</sub>). De même, GreenIT Analysis semble ne pas donner les mêmes résultats à chaque analyse, pour une même page.

![Test de la page d'accueil lacherez.info avec GreenIT Analysis](/asset/greenit_lacherez.png)

![Test de la page d'accueil lacherez.info avec GreenIT Analysis](/asset/websitecarbon_lacherez.png)

### Importance du choix du CMS dans la consommation côté client

Comme dit précédemment, il est facile de tester cet aspect côté client. Toutefois, il est beaucoup plus difficile d'estimer la part que peut jouer le CMS dans le "produit fini" (la page HTML), puisque le résultat dépend de beaucoup d'autres choses (contenu du site, design graphique du template). Il n'y aurait évidemment aucun sens à comparer un site de démo ou un site vide avec un site en production proposant. L'idéal serait de comparer le même site implémenté avec plusieurs CMS différents, ce qui n'est naturellement guère possible.

Afin d'avoir une idée *a minima* de l'importance du CMS sur cet aspect, nous proposons de comparer des versions de démo des CMS proposées par [Softaculous](https://www.softaculous.com/). Il ne s'agit pas à proprement parler de versions de démo, mais simplement d'installations de base. Aussi imprécis que soit le résultat de ce test, il nous permettra d'avoir une idée sur l'écoconception "de base" des différents CMS (sans aucune personnalisation du contenu, des fonctionnalités utilisateur ou du thème) et de savoir si, d'emblée, l'un d'eux est évidemment moins performant.

![Test du site de démo de Grav sur Softaculous avec GreenIT Analysis](/asset/greenit_grav.png)

![Test du site de démo de Wordpress sur Softaculous avec GreenIT Analysis](/asset/greenit_wp.png)


![Test du site de démo de Grav sur Softaculous avec Website carbo](/asset/websitecarbon_grav.png)

![Test du site de démo de Wordpress sur Softaculous avec Website carbo](/asset/websitecarbon_wp.png)

Comme on peut le voir sur les captures d'écran ci-dessus, il n'y a pas de différence sensible, *a priori*, entre les pages générées par Grav et Wordpress.


Par conséquent, et comme on pouvait s'y attendre, le résultat côté client du choix du CMS n'est pas déterminant. Le choix des gabarits de pages (templates, thèmes...), lui, joue naturellement un rôle mais indépendant du CMS lui-même.

Ce qui reste en suspens, toutefois, c'est ce que ce choix change au niveau de la consommation du serveur. C'est à cette question que j'essaierai de répondre dans un prochain billet...
