# Uno

![](uno.jpg)

**Índice**

**[1. Reglas](#reglas)**

**[2. Modelo del dominio](#modelo-del-dominio)**
* [2.1 Diagramas de clases](#diagramas-de-clases)
    * [2.1.1 Simple](#simple)
    * [2.1.2 opcion 1](#opción-1)
    * [2.1.3 opcion 2](#opción-2)
* [2.2 Diagramas de objetos](#diagramas-de-objetos)
    * [2.2.1 Before Play Deck Details](#before-play-deck-details)
    * [2.2.2 Turn 0 - 3 players](#turn-0---3-players)

**[3. Diagrama de Actores y Casos de Uso](#diagrama-de-actores-y-casos-de-uso)**

**[4. Interfaz](#interfaz)**
  * [4.1 Componentes](#componentes)
  * [4.2 Ejemplo](#ejemplo)

**[5. Diseño](#diseño)**

**[6. Arquitectura](#arquitectura)**

# Reglas


### Como Jugar
https://youtu.be/DQzOvL87vss

### Manual de donde hemos sacado los nombres de las entidades.
https://www.unorules.com/

### Manual oficial en español
[](./Docs/Rules/UNO-reglas.pdf)

### Resumen del juego
https://www.unorules.org/

### Contenido de un Deck
![](https://www.unorules.org/wp-content/uploads/2021/03/All-Uno-cards-how-many-cards-are-in-a-uno-deck.png)


# Modelo del dominio
### Diagramas de clases
#### Simple
![](./Docs/DomainModel/DomainModel-Simple.png)

#### Opción 1
![](./Docs/DomainModel/DomainModel-Option1.png)

#### Opción 2
![](./Docs/DomainModel/DomainModel-Option2.png)

### Diagramas de objetos
#### Before Play Deck Details
![](./Docs/DomainModel/BeforePlay-DeckDetails.png)

#### Turn 0 - 3 players
![](./Docs/DomainModel/Turn0-3Players.png)

### Diagrama de Actores y Casos de Uso

| Diagrama de Actores y Casos de Uso | Diagrama de Contexto |
|---|---|
| ![](./Docs/Actores.png) | ![](./Docs/DomainModel/States.png)

# Interfaz


### Componentes
```
NumeredCardView:

    [Color, Number]
    
    Examples:
    [Red, 3]
    [Blue, 0]
    [Green, 9]
    [Yellow, 5]


ActionCardView:

    [Color, {Action}]
    
    Examples:
    [Red, DrawTwo]
    [Red, Skip]
    [Red, Reverse]


WildCardView:

    [{WildCardType}]
    
    Examples:
    [WildCard]
    [DrawFourWildCard]

PlayedWildCardView:

    [Color, WildCardType]
    
    Examples:
    [Green, WildCard]
    [Yellow, DrawFourWildCard] 


PlayerHandView:

    Player Number:
        1. CardView*,
        2. CardView - new!*
    
    Example:
    Player 1:
        1. [Red, 3],
        2. [WildCard] - new!
    Player 5:
        1. [Green, Reverse]


PlayerView:
    
    Player Number:
        [n cards]

    Examples:
    Player 1:
        [3 cards]
    Player 5:
        [1 card!]


TurnView:

    Current player:
        PlayerView
    Direction:
        [Forwards|Backwards]
    {DiscardPileView}
    
    Examples:
    Current player:
        Player 5:
            [3 cards]
    Direction:
        Forwards
    Discard pile: [Green, 0] at top
    Current player:
        Player 2:
            [1 card!]
    Direction:
        Backwards
    Discard pile: [Blue, Skip] at top
        

DrawPileView: (*no sabemos si hace falta)

    Draw pile: [n cards] remaining
    
    Example:
    Draw pile: [34 cards] remaining
    

DiscardPileView:

    Discard pile: {CardView} at top
    
    Example:
    Discard pile: [Green, 7] at top
    Discard pile: [Red, WildCard] at top


ColorsView:

    1. Blue
    2. Red 
    3. Green
    4. Yellow


SelectColorActionView: (*después de usar un comodín o comodín-roba-cuatro)

    What color to change to?
        {ColorsView}


PlayCardActionView: (* al principio de cada turno)

    What card you want to play?
        PlayerHandView

UnoStartView:

    UNO! Starting...
    How many players?
        [Read until correct answer, 2-10]
    How many human players?
        [Read until correct answer, 1 <= x <= players]
```


### Ejemplo
```
    UNO! Starting...
    How many players?
        -> 0
    Invalid answer! Player must be inbetween 2 and 10.
    How many players?
        -> 3
    3 players selected.
    How many human players?
        -> 4
    Invalid answer! Human players must be lower than total players.
    How many human players?
            -> 0
        Invalid answer! Human players must be at least 1.
    
    ===============
    
    Shuffling deck.
    Players hands dealt.

    Player 1:
            1.[Red, 3], - new!
            2.[Green, 4], - new!
            3.[Green, Reverse], - new!
            4.[Red, 0], - new!
            5.[Yellow, Skip], - new!
            6.[WildCard] - new!
            7.[WildCard] - new!
    ...
    
    ================
    
    Current player:
        Player 1:
            [7 cards]
    Direction:
        Forwards
    Discard pile: [Blue, 3] at top
        
    What card you want to play?
        Player 1:
                1.[Red, 3], - new!
                2.[Green, 4], - new!
                3.[Green, Reverse], - new!
                4.[Red, 0], - new!
                5.[Yellow, Skip], - new!
                6.[WildCard] - new!
                7.[WildCard] - new!
        -> 8
        Invalid answer! Card out of range.
    What card you want to play?
            Player 1:
                    1.[Red, 3], - new!
                    2.[Green, 4], - new!
                    3.[Green, Reverse], - new!
                    4.[Red, 0], - new!
                    5.[Yellow, Skip], - new!
                    6.[WildCard] - new!
                    7.[WildCard] - new!
            -> 1
    ================ 
    ...
    ...
    ...        
```


# Diseño

Ver apartado [Design](./Docs/Design/ReadMe.md).

# Arquitectura

Ver apartado [Architecture](./Docs/Architecture/ReadMe.md).