### BEAUCHET Quentin - FORNER YANN - Groupe 1

# Installation

## Prérequis

- Python 3
- Mosquitto (Broker MQTT)
- Paho (Client MQTT pour Python) `pip install paho-mqtt`

## V1

```c
1. Lancer webots
2. File / Open world
3. Ouvrir khepera3.wbt
4. Selectionner le controller de Khepera2
5. Choisir v1
```

## V2

```c
1. Lancer webots
2. File / Open world
3. Ouvrir khepera3.wbt
4. Selectionner le controller de Khepera2
5. Choisir v2
6. Lancer la commande 'mosquitto -p 1880' depuis le dossier components
7. Lancer la commande 'docker-compose up -d' depuis le dossier components
```

# Choix algorithmiques

Nous avons choisis d'implementer une coordination basé sur des poids attribués a chaque comportement.

La coordinations basé sur des votes nous aurait empeché d'utiliser l'agorithme de braitengerg car les consignes aurait été trop precises et donc trop nombreuses. En revanche nous implementons tout du meme un systeme de veto car des lors que braitengerg detecte un obstacle il prend la priorité sur la recherche de source lumineuse.

Nous avons fait le choix de stopper la recherche de sources lumineuse des lors que la luminosité maximum detectés par un capteur passe sous un seuil predefini.

Notre comportement final est donc celui d'un robot parcourant son environement tout en evitant les obstacles jusqu'a ce que celui ci decouvre une source lumineuse. Tel un papillion dans le noir, il se retrouve alors a tourner autour de celle ci a pour toujours.

A noter que le fichier de coordination à été rendu générique pour pouvoir minimiser le nombre de choses à modifier en cas d'ajout d'un nouveau module de comportement.

En migrant l'application sur Docker nous nous somme rendu compte que le choix de MQTT n'etait peut etre pas le plus adaptés car des lors que la vitesse du simulateur est trop grande, les communications n'arrivent plus a suivre et le robot se retrouve a executer des ordres datant d'il y a trop longtemps ce qui lui donne un comportement inatendu.

# Examples

```python
braitengerg = [10,10]  #Pas d'obstacle
light = [0,0]          #Pas de lumiere
speed = [10,10]        #Tout droit
```

```python
braitengerg = [10,10]  #Pas d'obstacle
light = [5,10]         #Detection de lumiere
speed = [7.5,10]       #Deplacement du robot vers la source lumineuse
```

```python
braitengerg = [5.3,10] #Detection d'un obstacle
light = [10,5]         #Detection de lumiere
speed = [10,10]        #L'evitement de l'obstacle prend la priorité
```

# Architecture

On peut voir ci dessous le schéma de notre architecture, avec les différents topics par lequel passe nos données ainsi qu'un exemple de la donnée type qui va passer dedans. Il n'y a pas de différence entre les flèches bleues et rouges, à part que les rouges sont des messages issus de calculs et transporte l'information de la puissance à passer dans les roues. Alors que les flèches bleues signifient que l'on apporte les données brutes venant des capteurs.

![alt text](https://github.com/Yann-Forner/tp4_webots/blob/main/architecture.png?raw=true)
