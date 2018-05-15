---
layout: post
title: Passer de Wordpress à Jekyll
tag:
featured-img: wp2jekyll
---
Il y avait longtemps que j'avais envie d'essayer [Jekyll](https://jekyllrb.com/). C'est fait ! J'ai migré ce blog de [Wordpress](https://fr.wordpress.org/) vers [Jekyll](https://jekyllrb.com/)(vous pouvez, pendant encore quelque temps, consulter [la version Wordpress de ce blog](http://liltools.lacherez.info/), qui naturellement ne sera plus mise à jour).

XXX Article sur Jekyll : https://guillaumebriday.fr/de-wordpress-a-jekyll

Je consigne ici quelques notes sur la démarche que j'ai utilisée pour la migration.

# Récupération des billets du blog
Jekyll propose [un outil spécifique, appelé Jekyll-import pour importer dans Jekyll des contenus provenant de différents CMS et plateformes de blog](https://import.jekyllrb.com/docs/home/). Naturellement, cet outil propose [une méthonde pour faire l'import depuis Wordpress](https://import.jekyllrb.com/docs/wordpress/), mais il est prévu pour se connecter directement à la base de données du Wordpress. Mon Jekyll n'est pas installé chez le même hébergeur que mon Wordpress, aussi, il aurait fallu que je fasse un dump de ma base de données, que je l'importe ailleurs (sur ma machine perso, par exemple) avant de faire la migration. Rien d'inabordable, mais je suis trop paresseux :) J'ai donc préféré utiliser le [connecteur pour RSS](https://import.jekyllrb.com/docs/rss/). Le fil RSS du blog est accessible en ajoutant `feed` à la fin de l'adresse du blog (<http://liltools.lacherez.info/feed> en l'occurrence).

Toutefois, `jekyll-import` utilise l'élément `<description>` de chaque billet présent dans le fichier RSS. Or cet élément contient, comme on peut s'en douter, uniquement un résumé du billet, le contenu complet étant dans l'élément `<content:encoded>`. J'ai donc modifié la fonction `self.process()` du fichier `jekyll-import-0.14.0/lib/jekyll-import/importers/rss.rb` (dans le répertoire où sont stockés les gems ruby). La modification ne porte que sur la dernière ligne de la fonction (avant les `end`, à la ligne 59 du fichier)

```ruby
 # [...]
 # Process the import.
 #
 # source - a URL or a local file String.
 #
 # Returns nothing.
 def self.process(options)
   source = options.fetch("source")

   content = ""
   open(source) { |s| content = s.read }
   rss = ::RSS::Parser.parse(content, false)

   raise "There doesn't appear to be any RSS items at the source (#{source}) provided." unless rss

   rss.items.each do |item|
     formatted_date = item.date.strftime("%Y-%m-%d")
     post_name = item.title.split(%r{ |!|/|:|&|-|$|,}).map do |i|
       i.downcase if i != ""
     end.compact.join("-")
     name = "#{formatted_date}-#{post_name}"

     header = {
       "layout" => "post",
       "title"  => item.title,
     }

     header["tag"] = options["tag"] if !options.to_s.empty?

     FileUtils.mkdir_p("_posts")

     File.open("_posts/#{name}.html", "w") do |f|
       f.puts header.to_yaml
       f.puts "---\n\n"
       #f.puts item.description # Ancienne ligne
       f.puts item.content_encoded # Nouvelle ligne
     end
   end
 end
```

# Récupération des images du blog

L'import RSS ne permet pas de récupérer les images du blog Wordpress (l'import Wordpress non plus d'ailleurs), il m'a donc fallu trouver une solution pour les images. J'ai fait le choix de simplement rechercher dans les contenus des billets les références aux images, de télécharger ces images dans un répertoire spécifique et de récrire les références avec cette nouvelle adresse. A noter que certaines de mes images, pour une raison que je ne connais pas (peut-être qu'un jour ce blog a été hébergé sur [wordpress.com](https://wordpress.com)), avaient une adresse en wp.com.

Le script que j'ai écrit pour cet usage se trouve sur [Github](https://github.com/blacherez/recupimages), j'en parlerai dans un prochain billet. 

# Images de couverture

```bash
 $ ls|sed 's/\(.*\)\.\(.*\)/convert \1.\2 -resize 535 \1.jpg/'
 $ ls *.*|sed 's!\(.*\)\.\(.*\)!convert \1.\2 images/\1.jpg!'
 ```
TODO : ajouter un .htaccess pour ne pas perdre les liens
