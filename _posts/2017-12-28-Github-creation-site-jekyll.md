---
layout: post
title: GitHub / Un blog Jekyll en 5mn chrono 
tags: [Git, GitHub, Linux]
---

## Fork de Jekyll-now

Forker le repository GitHub suivant, sous le nom "votrenom.github.io" :
[https://github.com/barryclark/jekyll-now](https://github.com/barryclark/jekyll-now)

Le site est presque immédiatement visible sur https://votrenom.github.io

On peut rajouter des posts en créant des fichiers au format Markdown dans le dossier `_posts` (s'inspirer de la syntaxe du fichier existant).

## Développement local

Pour pouvoir développer le site web en local, on doit cloner le répository précédemment créé, puis installer quelques packages :

```bash
sudo apt-get install ruby ruby-dev
sudo gem install bundler
```

A la racine du repository local, on crée un fichier `Gemfile` contenant :

```text
source 'https://rubygems.org'
gem 'github-pages', group: :jekyll_plugins
```

```bash
bundle install
```

Enfin, on lance Jekyll en mode `--watch` :

```bash
bundle exec jekyll serve --watch
```

Le site est visible à l'adresse [http://127.0.0.1:4000/](http://127.0.0.1:4000/) et se met automatiquement à jour à chaque modification (pas besoin de relancer Jekyll).