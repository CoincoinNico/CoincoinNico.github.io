---
layout: post
title:  "Tuto Flappy Bird (15min) – débuter avec Gosu"
date:   2014-04-24 10:08:54
categories: jekyll update
---
![](./../../../../../media/flappy.png)
# Les trucs chiants pour commencer (7 minutes)
## 1. Téléchargez la gem Gosu

```ruby
$ gem install gosu
```

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

- `super(width, height, fullscreen)` et `game_window = GameWindow.new(800, 600, false)` : crée une fenêtre de 800x600, pas en plein écran

- `update` : un des trucs trop cool de Gosu : 60 fois par seconde la méthode update
  est appelée par défaut ce qui permet de créer des animations. Ainsi écrire x += 1 fait que la position x qui était à zéro sera une seconde plus tard à 60. Votre personnage s'est déplacé. Toute la logique est ici.

- `draw` : tout le « design » est dans draw.

![drawing](./../../../../../media/background.png)
## 3. Insérer l'image des nuages en fond d'écran

Téléchargez cette [image](./../../../../../media/background.png) et mettez là dans un dossier media en l'appelant "background.png".

```ruby
require 'gosu'
class GameWindow < Gosu::Window
  def initialize
    # votre ancien code est ici[...]
    @background_image =  Gosu::Image.new(self, "media/background.png", true)
  end
  # votre ancien code est ici[...]
  def draw
    @background_image.draw(0, 0, 0)
  end
end
window = GameWindow.new
window.show
```

- `@background_image =  Gosu::Image.new(self, "media/background.png", true)` : prend trois arguments : la fenere dans laquelle l'image apparaît, le fichier de l'image, le troisième pour savoir si vous voulez que les bordures de l'image soient "hard" … à voir avec Gab

- `@background_image.draw(0, 0, 0)` : comme dit précédemment, ici on met le design : les trois arguments sont x, y et z. occupez-vous surtout de x et y, l'image commence donc à (0,0)

![](./../../../../../media/flappy.png)
#4. Créez l'oiseau dans un nouveau ficher bird.rb

Téléchargez cette [image](./../../../../../media/flappy.png) et mettez là dans le dossier media en l'appelant "flappy.png".

```ruby
require 'gosu'

class Bird
  attr_accessor :x, :y

  def initialize(window)
    @image = Gosu::Image.new(window, "media/flappy.png", false)
    @angle = 0
    @gravity = 5
    @x, @y = x, y
  end

  def update
    @angle += 2
    @angle = [@angle,90].min
    @y += @gravity
  end

  def draw
    @image.draw_rot(@x, @y, 1, @angle)
  end
end
```

Très similaire à game.rb si vous remarquez :-)

- `@image = Gosu::Image.new(window, "media/flappy.png", false)` : idem à background, media/flappy.png est le dessin d'un oiseau flappy bird
- `@angle = 0, @gravity = 5, @x, @y = x, y` : On définit un angle : l'orientation de l'oiseau. Une gravité (simplement pour l'instant un nombre : à chaque itération (60 par seconde), l'oiseau sera attiré de @gravity vers le bas.
- `@angle += 2, @angle = [@angle,90].min, @y += @gravity` : au hasard, on tâtonne, +2 à chaque itération (+2 le fait s'orienter vers le bas).
Y est à 0 en haut de la fenêtre, et au max en bas. On augmente ainsi de @gravity la position de y


#5. Relier le fichier bird.rb et game.rb pour que l'oiseau apparaisse sur notre fond d'écran : on modifie le fichier game.rb et un peu bird.rb

game.rb
```ruby
require_relative 'bird.rb'
#some code goes here[…]
  def initialize
    #some code goes here[…]
    @bird = Bird.new(self)
    @bird.warp(320, 240)
  end

  def update
    @bird.update
  end

  def draw
    […]
    @bird.draw
  end
```

bird.rb
```ruby
def (wrap(x,y)
 @x, @y = x, y
end
```

- `@bird = Bird.new(self)` crée mon oiseau
- ` @bird.wrap(320, 240)` à relier avec `wrap(x,y)` dans bird.rb : permet à l'oiseau de commencer à width = 320 et height = 240.
- `@bird.update` et `@bird.draw` lancent les méthodes update et draw de bird.rb

*** LANCEZ LE PROGRAMME ***
*** VOUS AVEZ UN OISEAU QUI TOMBE:-) ***




