# 3. Séquence de test

Source : `logiciel/spec-soft-test-bp-v4.doc` (v4.0 du 24/02/2015, N. Gille), transcrite dans [`logiciel/spec-soft-test-bp-v4.md`](../logiciel/spec-soft-test-bp-v4.md).

Cette séquence est celle du logiciel du dsPIC de la carte d'interface. Le code lui-même n'est pas dans les archives.

```mermaid
flowchart TD
  R[Presse fermée, carte présente<br/>levée du reset] --> S{Strap de forçage<br/>en place ?}
  S -- non --> W[TEST FINI clignote rouge] --> S
  S -- oui --> D[24 V seul, attente 21 s]
  D --> A1[Mesures sous secteur<br/>15V 15VS 5VS 3V3S 15VSP 12VSP]
  A1 --> P1[Pilotage 1/2 : +15VSP présent]
  P1 --> A2[12 V seul<br/>mesures 15VS 5VS 3V3S]
  A2 --> P2[Pilotage 2/2 : attente 6,5 s<br/>+15VSP à 0 V]
  P2 --> F1[Forçage 1 : +15VSP à 0 V<br/>F clignote rouge]
  F1 --> F2{Strap retiré ?}
  F2 -- oui --> F3[Forçage 2 : +15VSP présent]
  F3 --> AL[24 V seul<br/>alims alarme]
  AL --> BA1[Alim bus BA]
  BA1 --> BA2[Réponse aux trames BA<br/>en permanence]
```

## Étapes

| # | Étape | Action du banc | Voyant |
|---|---|---|---|
| 1 | Reset | Le dsPIC est en reset tant que le palpeur n'est pas enfoncé. La carte testée n'est pas alimentée. | – |
| 2 | Lancement | Tant que le strap de forçage +15VSP est « enlevé » (entrée ouverte), le test n'avance pas. | TEST FINI clignote rouge |
| 3 | Démarrage | Strap « mis » : alimentation en **24 V seul**, attente de **21 s**. | TEST FINI éteint |
| 4 | Alims sous secteur | Mesure de 15V, 15VS, 5VS, 3V3S, +15VSP, +12VSP. | Un voyant par tension, ligne « Alim secteur » |
| 5 | Pilotage 1/2 | +15VSP doit être présent. | – |
| 6 | Alims sous secouru | Application du 12 V **puis** coupure du 24 V. Mesure de 15VS, 5VS, 3V3S. | Un voyant par tension, ligne « Alim secourue » |
| 7 | Pilotage 2/2 | Attente que le BP coupe le +15VSP (**6 500 ms**, valeur prise pour le firmware 092-31 en mode test). +15VSP doit être à 0 V flottant. | **P** vert si 5 et 7 bons, rouge sinon |
| 8 | Forçage 1 | +15VSP doit être à 0 V flottant. | **F** clignote rouge : l'opérateur doit retirer le strap |
| 9 | Forçage 2 | Attente sans limite du retrait du strap, puis +15VSP doit être présent. | **F** vert si 8 et 9 bons, rouge sinon |
| 10 | Alims alarme | Application du 24 V **puis** coupure du 12 V. Mesure des alimentations : détecteur de présence 1, détecteur de présence 2, sirène intérieure, sirène extérieure. | Un voyant par alimentation |
| 11 | Alim bus BA | Mesure de l'alimentation du bus BA. | Voyant « Boîtiers auxiliaires » |
| 12 | Communication BA | Le banc attend en permanence une trame BA « pièces de vie » (PDV) et y répond. Le BP lance le scénario « Personnalisé » ; il analyse lui-même le résultat et l'affiche sur son IHM. | – |
| 13 | Fin | Retirer la carte relâche le palpeur et remet le dsPIC en reset. Une nouvelle carte relance tout. | – |

Règle commune aux mesures : voyant **vert** si la mesure est bonne, **rouge** sinon.

## Ordre des commutations d'alimentation

La spec insiste sur l'ordre, pour ne jamais laisser la carte sans alimentation :

- passage en secouru : appliquer le 12 V **d'abord**, couper le 24 V **ensuite** ;
- retour en secteur : appliquer le 24 V **d'abord**, couper le 12 V **ensuite**.

## Le strap de forçage

Le strap de forçage du +15VSP est un bouton à deux voies : une voie fait le strap lui-même, l'autre renvoie son image au dsPIC sur l'entrée « palpeur ». L'opérateur le met au début et le retire quand **F** clignote.

## Mesures analogiques

L'annexe de la v4 est un tableau manuscrit : `etude/mesures-analogiques-annexe-spec-v4.pdf`. Pour chaque tension, il donne la valeur réelle, la valeur après le pont diviseur, la valeur binaire et la valeur hexadécimale lues par le convertisseur 12 bits. En noir : valeur nominale mesurée. En rouge : seuil.

Lignes du tableau : 15V, 15VS, 5VS, 3V3S, 15VSP, 12VSP, 15VS secouru, 5VS secouru, 3V3S secouru, 15VSP secouru, 12VSP pilotage, DPR1 ALIM, DPR2 ALIM, SINT ALIM, SEXT ALIM, BA ALIM.

> Le tableau n'est pas retranscrit ici : l'écriture est difficile à lire et une erreur de seuil serait pire que l'absence de valeur. À retranscrire par quelqu'un qui peut le comparer au banc.

## Signal des sirènes

`etude/calculs-test-signal-sirenes-v1.xlsx` calcule le pont diviseur du test de la commande de sirène sous 15 V :

| Résistance série | Résistance de charge | Tension | Courant |
|---|---|---|---|
| 4 700 Ω | 4 700 Ω | 7,5 V | 1,6 mA |
| 10 000 Ω | 4 700 Ω | 4,8 V | 1,0 mA |
