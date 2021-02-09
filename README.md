# Hindernislauf Einführung


## ~ @unplugged
Es soll ein Hindernislauf programmiert werden.
Ziel des Spieles: Sammle möglichst viele Punkte indem du mit den Tasten A und B den Hindernissen ausweichst, welche vom oberen Bildschirmrand nach unten bewegt werden.
![Hindernislauf](https://github.com/r00b1nh00d/Spiele_Programmieren_Lernen_Hindernislauf/blob/master/HindernislaufGIF.gif?raw=true)


## Schritt 1: Erstelle eine Spielfigur
Als erstes erstellen wir uns eine ``||Variables: Variable||`` namens ``||Variables: Spielfigur||``. Diese kommt in den Block ``||basic: Beim Start|``.
```blocks
let Spielfigur: game.LedSprite = 0

```

## Schritt 2: Einen Sprite auf die Variable speichern
Im Bereich ``||game: Spiel||`` was unter ``|advanced: Fortgeschritten|`` wird mit den sogenannten Sprites gearbeitet. Diese Sprites sind wie Spielsteine oder Spielfiguren
Sie besitzen eine Position (eingeteilt in X von links nach recht und Y von oben nach unten), eine Blickrichtung, eine Helligkeit und sie können blinken.  <br>
Nimm nun aus dem Bereich ``||game: Spiel||`` den Block ``||game: erzeuge Sprite an Position x y ||``. Dieser wird in den Block ``||Variables: Setze Variable auf||`` geschoben.
Jetzt sollte im Simulator eine LED in der Mitte des Calliope Displays leuchten, eben an der Position x=2 (ganz links ist 0 und ganz rechts ist 4) und y=2 (ganz oben ist 0 und ganz unten ist 4). Ändere nun den Y-Wert auf 4 damit die Spielfigur am unteren Bildschirmrand erscheint.

```blocks 
let Spielfigur = game.createSprite(2, 4)
```

## Schritt 3: Bewegen der Spielfigur
Lass die Spielfigur mit dem A-Knopf ein Feld nach links und mit dem B-Knopf ein Feld nach rechts bewegen. <br>
Dazu benötigst du aus dem Bereich ``||Input: Eingabe||`` den Block ``||Input: Wenn Knopf A gedrückt||``, in diesen kannst du den Block ``||game: Sprite bewege um 1 ||`` hineinschieben. <br>
Wird nun der Knopf A gedrückt sollte sich deine Spielfigur um eins nach rechts bewegen. Um die spielfigur nach links zu bewegen kannst du sie um -1 bewegen.
Die Spielfigur bewegt sich auf der x-Achse, da sie eben in diese Richtung ausgerichtet ist. Dies ist für den späteren Hindernislauf richtig. <br>
Testweise kannst du ``||basic: Beim Start|`` den Block ``||game: drehe rechts um 90° ||`` einschieben um die Spielfigur hoch und runter zu bewegen. 

```blocks


input.onButtonPressed(Button.A, function () {
    Spielfigur.move(-1)
})

input.onButtonPressed(Button.B, function () {
    Spielfigur.move(1)
})

let Spielfigur = game.createSprite(2, 4)
```

## Schritt 4: Das erste Hindernis am Horizont
Wir brauchen ein Hindernis. Hierfür müssen wir eine neue Variable erstellen und einen neuen Spielspielstein erzeugen. Weißt du noch wie es geht? 
Hier ein kleiner Tipp wo du alles findest: ``||Variables: Setze Variabele Hindernis auf||`` und ``||game: erzeuge Sprite an Position x y||``<br>

```blocks
input.onButtonPressed(Button.A, function () {
    Spielfigur.change(LedSpriteProperty.X, -1)
})
input.onButtonPressed(Button.B, function () {
    Spielfigur.change(LedSpriteProperty.X, 1)
})
let Spielfigur: game.LedSprite = null
Spielfigur = game.createSprite(2, 4)
let Hindernis = game.createSprite(randint(0, 4), 0)
basic.forever(function () {
	
})
```

## Schritt 5: Das Hindernis setzt sich in Bewegung
Nun muss sich unser Hindernis nach unten auf die Spielfigur zubewegen. Hierfür können wir dem Hindernis einfach sagen, dass er sich 4 mal nach unten bewegen soll (``||game: ändere y um 1||``). 
Am Besten machen wir dass, indem wir eine Schleife nutzen (``||loops: wiederhole 4 mal||``). Damit das nicht so schnell geht, benötigen wir noch eine Pause (``||basic: pausiere 100ms||``).<br>

```blocks
input.onButtonPressed(Button.A, function () {
    Spielfigur.change(LedSpriteProperty.X, -1)
})
input.onButtonPressed(Button.B, function () {
    Spielfigur.change(LedSpriteProperty.X, 1)
})
let Spielfigur: game.LedSprite = null
Spielfigur = game.createSprite(2, 4)
let Hindernis = game.createSprite(2, 0)
basic.forever(function () {
	for (let index = 0; index < 4; index++) {
        Hindernis.change(LedSpriteProperty.Y, 1)
        basic.pause(100)
    }
    Hindernis.set(LedSpriteProperty.Y, 0)
    basic.pause(100)
})
```

## Schritt 6: Das Hindernis wird zur Gefahr
Wenn das Hindernis unten angekommen ist, müssen wir noch überprüfen ob es die Spielfigur trifft. Hierfür nutzen wir aus der Logik 
``||logic:Wenn dann sonst||`` Bedingung. Als Bedingung nutzen wir ``||game: Sprite berührt ||``. Hier tragen wir unser Hindernis und unsere Spielfigur ein.
In den Dann-Bereich (also wenn das Hindernis die Spielfigur berührt) beenden wir das Spiel mit ``||game: Spiel beendet ||`` und in dem Ansonsten-Bereich
setzen wir das Hindernis wieder nach oben (``||game: Sprite setze y auf 0||``) und geben einen Punkt (``||game: ändere Spielstand um 1||``).
```blocks
input.onButtonPressed(Button.A, function () {
    Spielfigur.change(LedSpriteProperty.X, -1)
})
input.onButtonPressed(Button.B, function () {
    Spielfigur.change(LedSpriteProperty.X, 1)
})
let Spielfigur: game.LedSprite = null
Spielfigur = game.createSprite(2, 4)
let Hindernis = game.createSprite(2, 0)
basic.forever(function () {
	for (let index = 0; index < 4; index++) {
        Hindernis.change(LedSpriteProperty.Y, 1)
        basic.pause(200)
    }
    if (Spielfigur.isTouching(Hindernis)) {
        
        game.gameOver()
    } else {
        Hindernis.set(LedSpriteProperty.Y, 0)
        game.addScore(1)
    }
    basic.pause(200)
})
```

## ~avatar avatar @unplugged
Unter : [https://github.com/r00b1nh00d/Spiele_Programmieren_Lernen_Hindernislauf/blob/master/KurzHilfeSpiele.pdf](https://github.com/r00b1nh00d/Spiele_Programmieren_Lernen_Hindernislauf/blob/master/KurzHilfeSpiele.pdf) <br>
findest du auch nochmal eine kurze Übersicht zu den Befehlen aus dem Bereich ``||game: Spiele||``. <br>
Jetzt ist es an dir dieses Tutorial zu verlassen und in den Editor zu wechseln. <br>
Füre dort die Hindernisse 1 bis 3 dem Programm hinzu und lass diese ``||basic: Dauerhauft||`` und immer wieder neu auf der Anzeige des @boardname@ von oben nach unten bewegen. <br>
Berührt ein Hindernis die Spielfigur ist das Spiel vorbei. Sind die Hindernisse am unteren Rand angelangt sollen sie erneut an ``||maths:zufälliger||`` Stelle auftauchen und ein Punkt vergeben werden. Über eine Zeiteinstellung mittels ``||basic:pausiere||`` kannst du die Schwierigkeit einstellen. 




## Als Tutorial verwenden

Dieses Repository kann als **Tutorial** für MakeCode verwenden.

öffne dazu den Link: [https://makecode.calliope.cc/#tutorial:https://github.com/r00b1nh00d/Spiele_Programmieren_Lernen_Hindernislauf]
#### Metadaten (verwendet für Suche, Rendering)

* for PXT/calliopemini
<script src="https://makecode.com/gh-pages-embed.js"></script><script>makeCodeRender("{{ site.makecode.home_url }}", "{{ site.github.owner_name }}/{{ site.github.repository_name }}");</script>
