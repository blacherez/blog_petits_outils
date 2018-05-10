---
layout: post
title: Ouvrir un lien dans une nouvelle fenêtre avec markdown
tag:
featured-img: fenetre
---
Je ne suis généralement pas très amateur de liens qui s'ouvrent dans une nouvelle fenêtre ou un nouvel onglet, il me semble que c'est à l'utilisateur de décider s'il veut remplacer la page qu'il est en train de lire ou s'il veut ouvrir un nouvel onglet. Toutefois, il me semble plus logique, parfois, de le faire : par exemple, si j'utilise un mot en le liant à la page Wikipedia correspondante, l'ouverture d'un nouvel onglet me semble s'apparenter à une note de bas de page.

En tout cas, ce comportement n'est pas prévu dans le Markdown standard, mais certains moteurs acceptent la syntaxe `{:target="_blank" }`. C'est le cas de `kramdown`, le moteur par défaut utilisé dans Jekyll. Ainsi, le code suivant :

```markdown
[Ma page personnelle](https://lacherez.info){:target="_blank" }
```
donne :
[Ma page personnelle](https://lacherez.info){:target="_blank" }

Toutefois, comme cette syntaxe n'est pas standard, elle perturbe la coloration syntaxique (en tout sur [atom](https://atom.io)).

Crédit photo : [Ségur le Château, Maison Henri IV, fenêtre](https://commons.wikimedia.org/wiki/File:S%C3%A9gur_le_Ch%C3%A2teau_maison_Henri_IV_fen%C3%AAtre.JPG) Père Igor [GFDL (<http://www.gnu.org/copyleft/fdl.html>) ou CC BY-SA 3.0 (<https://creativecommons.org/licenses/by-sa/3.0>)], de Wikimedia Commons
