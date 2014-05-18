---
layout: post
title:  "Les routes de A à Z (1heure)"
date:   2015-05-16 17:48:00
categories: jekyll update
---
![](./../../../../../media/flappy.png)
# Rails - Routes
## 1. Généralités
Il faut vous référer à mon post pour débuter avec Rails.

## 2. Avoir singles/profile au lieu de singles/profile/:id

Si jamais vous ne voulez pas avoir le numéro d'id dans l'url il y a une solution ! Tout mettre au singulier !

``` resource :profile ```

Mais attention ! Un bug de Rails empêche alors form_for de fonctionner correctement (voir le post "Débuter avec Rails" pour form_for). Il convient alors d'écrire:

``` form_for @profile, url: profile_path do |f| ```

## 3. Gérer les routes des commentaires écrits sur les célibataires

On appelle cela les 'Nested Resources', cela inclue les groupes d'éléments qui sont enfants d'une autre ressource. Ainsi si je permets à mes utilisateurs de commenter les profils des célibataires, j'ai une relation entre ma table "comments" et la table "singles": un commentaire à un seul célibataire mais un célibataire a plusieurs commentaires.

C'est tout bête !

On utilise:

``` ruby
resources :singles do
  resources :comments
end
```

De manière classique et magique (exactement comme dans le post "Débuter avec Ruby on Rails"), nous avons les 7 actions de créées:

Nous avons alors 7 actions qui n'ont besoin que de 4 différentes URL comme les verbes http sont différents:

```GET``` de l'URL ```/singles/:single_id/comments``` appelle la méthode ```index``` et liste tous les commentaires pour le célibataire concerné

```POST``` de l'URL ```/singles/:single_id/comments``` appelle la méthode ```create``` et crée un nouveau commentaire sur le célibataire sélectionné

```GET``` de l'URL ```/singles/:single_id/comments/new``` appelle la méthode ```new``` et renvoie un formulaire pour créer un nouveau commentaire sur le célibataire concerné (la soumission de ce formulaire appelle ensuite ```/singles/:single_id/comments```)

```GET``` de l'URL ```/singles/:single_id/comments/:id``` appelle la méthode ```show``` et affiche un commentaire unique unique

```DELETE``` de l'URL ```/singles/:single_id/comments/:id``` appelle la méthode ```destroy``` et supprime ce commentaire

```PATCH/PUT``` de l'URL ```/singles/:single_id/comments/:id``` appelle la méthode ```update``` et update un commentaire

```GET``` de l'URL ```/singles/:single_id/comments/:id/edit``` appelle la méthode ```edit``` et renvoie un formulaire pour éditer le commentaire (la soumission de ce formulaire appelle ensuite ```PATCH/PUT /singles/:single_id/comments/:id```)

Cela va aussi créer des routing helpers comme edit_single_comment_path qui vont prendre comme paramètre une instance de Single: ```edit_single_comment_path(@single)```. Ainsi nous faisons cela au lieu de spécifier l'ID. Les deux sont acceptables. Si nous voulions utiliser l'id, nous aurions :

DEMANDER A SEBASTIEN :-)


GOOD PRACTICE : NE JAMAIS AVOIR DES RESSOURCES QUI ONT DES GRANDS-PARENTS ;-P

Mais alors comment je fais moi si je veux ajouter des réponses aux commentaires ?!

(section updatee prochainement, mais là pas besoin pour l'instant!)

## 4. "Quest-ce que signifie apprivoiser ?" "Cela veut dire 'créer des liens'".

Attention on s'accroche car si c'est magique, on va comprendre...

Alors on en est là:

```ruby
resources :singles do
  resources :comments
end
```

On sait en faisant ```rake routes``` et en regardant les prefix, qu'est alors créé single_comment_path qui peut prendre à la place de l'id du célibataire ET de l'id du commentaire, des instances de Single et de Comment:

```ruby
<%= link_to 'Comment', single_comment_path(@single, @comment) %>
```

Si vous voulez juste un lien vers le célibataire:

```ruby
<%= link_to 'Single', @single %>
```

Et pour les autres actions que show ? Alors il faut indiquer l'action comme le premier élément du tableau :

```ruby
<%= link_to 'Edit Comment', [:edit, @single, @comment] %>
```

Top non ?! Maaaaagique ! Et simple !

## 5. Si je veux me faire chier et ne pas utiliser resources

Pourquoi ? C'est lourd les ressources et aussi apparemment il va être facile de mapper des "legacy URLs to new Rails actions" à bon entendeur (Séb?!)

### i. Only

Première chose, vous pouvez limiter les url créées grâce au only: :action

```ruby
resources :singles do
  resources :comments, only: [:index, :new, :create, :destroy]
end
```

### ii. La mécanique

#### Avec :controller


Définir une route c'est donner des symboles que Rails va traiter. La façon la plus rustique est d'expliciter le controller par :controller.

```ruby
get ':controller(/:action(/:id))'
```

Si une requête http de /singles/show/1 arrive jusqu'à cette route (cela veut dire qu'avant aucune des routes n'aura matché), alors l'action show du SinglesController sera appelée (assez magique déjà). Et params[:id] existe et vaut 1.

Qu'est-ce que sont les parenthèses ? Ce sont des paramètres optionnels. Ainsi une reqûete http /singles aurait aussi été mappée à SinglesController#index.

Et si la requête est /singles/show/1?comment_id=2 le controller SinglesController aura son action "show" qui sera appelée avec comme paramètres params[:id] à 1 et params[:comment_id] à 2 :-) Cette route aurait fait la même chose si la requête avait été /singles/show/1/2 :

```ruby
get ':controller/:action/:id/:comment_id'
```

#### Avec #


Vous pouvez cacher le :controller en utilisant la syntaxe magique du '#' comme nous l'avons appris en cours:

get 'singles/:id', to: 'singles#show'


to be continued...