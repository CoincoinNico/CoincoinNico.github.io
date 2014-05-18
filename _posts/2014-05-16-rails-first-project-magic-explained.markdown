---
layout: post
title:  "Tuto Rails (10heures) – créer son premier site   (1h) et le comprendre (9h)"
date:   2015-05-16 15:48:00
categories: jekyll update
---
![](./../../../../../media/flappy.png)
# Rails - Généralités
## 1. Rails n'est pas du Ruby
Il faut entendre par là que si vous connaissez Ruby et sa rigueur, n'essayez pas de l'appliquer pour débuter avec Rails. Traitez plutôt Rails comme un nouveau langage dans lequel du Ruby viendra se mettre.

Rails est un framework qui permet de passer de notre version locale de notre code à une version pouvant être mise sur la Toile. Cela demanderait une configuration très pointue si nous devions le faire nous-mêmes alors rails a fait le choix pour nous et vient avec plein de méthodes magiques qu'il faut utiliser sans se demander le Ruby qu'il y a derrière.

Le but de Rails est ainsi double: être plus productif que les langages classiques, et plus sympa avec ces méthodes au nom évoquateur. Rails vous cache toute la configuration et vous demande en contrepartie de comprendre les conventions qu'il impose.

Il m'a fallu du temps pour accepter ces méthodes magiques, mais en le faisant j'ai pu recréer mon site de rencontre en 3heures. Ce que nous allons faire à présent.

## 2. Les routes
### a. Généralités
Configuration cachée ! Rails reçoit des requêtes http et arrive à les traiter ! Qu'en fait-il ? Il identifie l'action d'un controller associée à cette URL (une action n'est ni plus ni moins qu'une méthode Ruby d'un controller).

Rails est aussi capable de créer dynamiquement des URLs et des liens entre les différentes sections de notre site.

### b. Exemples
Si je veux accéder au célibataire ``` GET /singles/1 ``` (requête http de type GET) alors Rails demande à son router de faire le lien avec l'action show du controller singles avec comme paramètre {id: 1} :

``` get 'singles/:id', to: 'singles#show' ```

Je n'explique pas ce qu'est :id (probablement un symbole) ni le "#" je laisse ça à la magie de Rails. Je n'ai pas codé le routeur, j'accepte donc cette syntaxe que j'apprends par coeur, et qui semble logique (comme nous le disions, Rails privilégie à la configuration la convention: si je respecte cette nomenclature, tout se passera bien).

Parenthèse "Le saviez-vous?". Si vous écrivez une string: 'singles#show' Rails s'attend en effet à un format controller#action mais si vous passez un symbole il va directement rooter vers une action!
``` get 'singles/:id', to: :show ```


### c. Choisir la simplicité - les 7 routes
Le plus simple lorsque l'on crée des routes est de connaître la syntaxe

``` resources :singles ```

qui crée les 7 routes pour les actions des controllers new, create (Crud), index, show(cRud), edit, update(crUd), destroy(cruD).


Nous avons alors 7 actions qui n'ont besoin que de 4 différentes URL comme les verbes http sont différents:

```GET``` de l'URL ```/singles``` appelle la méthode ```index``` et liste tous les célibataires

```POST``` de l'URL ```/singles``` appelle la méthode ```create``` et crée un nouveau célibataire

```GET``` de l'URL ```/singles/new``` appelle la méthode ```new``` et renvoie un formulaire pour créer le nouveau célibataire (la soumission de ce formulaire appelle ensuite ```POST /singles```)

```GET``` de l'URL ```/singles/:id``` appelle la méthode ```show``` et affiche un célibataire unique

```DELETE``` de l'URL ```/singles/:id``` appelle la méthode ```destroy``` et supprime ce célibataire (en couple ou mort!)

```PATCH/PUT``` de l'URL ```/singles/:id``` appelle la méthode ```update``` et update un célibataire

```GET``` de l'URL ```/singles/:id/edit``` appelle la méthode ```edit``` et renvoie un formulaire pour éditer le célibataire (la soumission de ce formulaire appelle ensuite ```PATCH/PUT /singles/:id```)

### d. Attention à l'ordre !!!
Rails lit les fichiers de haut en bas, ainsi si vous mettez une URL pour voir les amis d'un célibataire: ```GET /singles/:id/friends``` en dessous de votre ```resources :singles``` le chemin ```GET /singles/:id``` (correspondant à l'action show) sera appelé avant !

### e. Monsieur je comprends pas les _paths qu'on rajoute! C'est quoi ?
Dans vos vues html.erb souvent vous verrez quelquechose_path. Le quelque chose correspond au Prefix que vous voyez lorsque vous effectuer un ```rake routes```.

Dans le cas de notre ```ressources :singles``` nous avons:

singles_path qui retourne ```/singles```

new_single_path qui retourne ```/singles/new```

edit_photo_path(:id) qui retourne ```/singles/:id/edit``` et enfin

single_path(:id) qui retourne ```/singles/:id```

Alors pourquoi ? Nous avons dit qu'ils faisaient référence à PREFIX et non à l'url directement: ainsi vous pouvez modifiez les url dans le fichier de config sans avoir à aller partout dans le code modifier vos url. Conclusion: NE TOUCHEZ PAS AUX PREFIX ! Il est important qu'eux restent fixes. Et utilisez _path partout !

### f. Voilà! Vous savez tout sur les routes ! Si vous voulez aller plus loin...

i. définir plusieurs resources : resources :singles, :couples, :groups
ii.




crire moins de code que les langages classiques ce post nous allons écrire dans le fichier views/singles/show.html.erb ```
<%= render @single.comments %> ``` à la place de:

```ruby
<% @single.comments.each do |comment| %>
  <p>
    <strong>Comment:</strong>
    <%= comment.comment %>
  </p>
  <p>
    <%= comment.body %>
  </p>
<% end %>
```
Pourquoi cela marche ?!

1. Nous avons créé un fichier app/views/comments/_comment.html.erb qui contient:

```ruby
<p>
  <strong>Commenter:</strong>
  <%= comment.commenter %>
</p>

<p>
  <strong>Comment:</strong>
  <%= comment.body %>
</p>
```
Fichier que nous appelons grâce à la commande ```
<%= render @single.comments %> ```

2.


## 2. Dans un dossier, créez un fichier game.rb - ouvrez une fenêtre Gosu :

```ruby
require 'gosu'
class GameWindow < Gosu::Window
  def initialize(width, height, fullscreen)
    super(width, height, fullscreen)
    self.caption = "My own flappy !"
  end
  def update
  end
  def draw
  end
end
game_window = GameWindow.new(800, 600, false)
game_window.show
```
Explications:

-  `Gosu::Window` : la classe créée hérite de tous les paramètres de Gosu::Window






