# 7. Mode opératoire

Reconstitué à partir de la spécification du logiciel, de son code source et de la face avant. **Aucune notice d'utilisation n'a été retrouvée** : ce mode opératoire est une déduction, à valider sur le banc.

## Avant de commencer

- Le banc n'a pas été mis sous tension depuis des années. Contrôler d'abord, hors tension : l'état des clous et des ressorts, les câbles, les alimentations, et le fonctionnement du détecteur d'ouverture qui coupe le 230 V (voir [architecture](02-architecture.md)).
- Le firmware de la carte testée doit être en mode test. D'après les notes de version du logiciel du banc, il faut un BP en version 091-30 ou plus récente (temporisation de passage en secouru réduite en mode test).
- Le banc injecte du 230 V sur la partie fil pilote. Ne jamais ouvrir le coffret sous tension.

## Déroulé

1. Mettre le **strap de forçage** en position « mis » (bouton « Forçage alims pilotées »).
2. Poser la carte sur la plaque « 944-BP », brancher le câble Ethernet sur la carte.
3. Fermer la presse. Le test démarre seul.
   - Si TEST FINI clignote rouge : le strap n'est pas mis.
4. Attendre environ 21 s (démarrage de la carte), puis observer la ligne **Alim secteur** : 15V, 15VS, 5VS, 3V3S, 15VSP, 12VSP doivent passer au vert.
5. Observer la ligne **Alim secourue** : 15VS, 5VS, 3V3S au vert.
6. Le voyant **P** donne le résultat du pilotage du +15VSP.
7. Quand **F** clignote rouge, **retirer le strap**. F donne ensuite le résultat du forçage.
8. Observer les voyants d'alimentation des **détecteurs de présence** 1 et 2 et des **sirènes** intérieure et extérieure, puis celui des **boîtiers auxiliaires**.
9. Lire sur l'IHM de la carte le résultat du test de communication BA et du test Ethernet.
10. Tester à la main les autres fonctions avec les boutons et voyants de la face avant : détecteur d'ouverture, signal et fraude des détecteurs et des sirènes, fuite, pluie, vent, bouton magique, arrosage, fil pilote (zones jour et nuit, salles de bain 1 et 2), machines à laver, prises de sécurité, cumulus.
11. Ouvrir la presse et retirer la carte. Le banc repart de zéro à la carte suivante.

## Lecture des voyants

| Voyant | Vert | Rouge | Clignotant rouge |
|---|---|---|---|
| Tension (secteur, secouru) | Mesure dans la tolérance | Mesure hors tolérance | – |
| P | Pilotage du +15VSP bon | Défaut de pilotage | – |
| F | Forçage du +15VSP bon | Défaut de forçage | Retirer le strap |
| TEST FINI | – | – | Mettre le strap pour lancer le test |

Sur le tableau des alimentations, les cases barrées d'une croix n'ont pas de voyant : 230V, 15V, 15VSP, 12VSP, P et F n'existent que sur la ligne « secteur ».

Le croquis `etude/scan-tableau-test-alims.jpeg` précise pour le voyant 230V : « clignotant vert en cours, vert si on retrouve le 12 V appliqué sur ALIMPH / ALIMN, rouge sinon ».
