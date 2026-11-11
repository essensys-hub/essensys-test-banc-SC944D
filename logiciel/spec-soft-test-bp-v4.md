# Spécification — Soft de test BP (v4.0)

> Transcription du texte de `spec-soft-test-bp-v4.doc`, pour la recherche et la lecture en ligne. Le document Word fait foi. Orthographe d'origine conservée.
>
> Rédacteur, vérificateur, approbateur : N. Gille (Valentinéa). Document marqué propriété de Valentinea, non communicable sans accord.

## Révisions

| Version | Date | Commentaires |
|---|---|---|
| 1.0 | 02/01/2015 | Création. |
| 2.0 | 02/01/2015 | Rajout recommendations sur activation / désactivation alims UUT. Découpage du test bus BA en test alimentation + test communication. |
| 3.0 | 04/02/2015 | 1.7 plus précis. 1.8 plus précis. |
| 4.0 | 24/02/2015 | Rajout mesures analogiques en « annexes ». |

## 1. Reset

De façon hardware, le uC est en reset tant que le palpeur n’est pas enfoncé. Donc le uC n’est plus en reset lorsque le plapeur est enfoncé (= BDT fermé ET UUT présente). Le soft du BP démarre automatiquement dès la levée du reset.


## 2. Lancement du test

Pour l’instant la UUT n’est pas du tout alimenté (ni en 24V, ni en 12V).

L’entrée palpeur n’est pas utilisée pour le palpeur mais pour remonter une image du strap de forçage alim +15VSP (bouton 2 voies : 1 pour le strap lui même et 1 pour son image).

Suite à la levée du reset, tant que l’image du strap de forçage alim +15VSP est en position « strap enlevé » (= ETOR ouverte), on fait clignotter la LED « Test fini » en rouge et on ne va pas plus loin.

Dès que le strap de forçage alim +15VSP est en position « strap mis » (= ETOR fermée), on éteint la LED « Test fini » et on passe à la suite.


## 3. Démarrage UUT

Alimenter la UUT en 24V seul.

Attendre 21s que la UUT soit opérationnelle.


## 4. Test des alims sous secteur (application du 24V)

Mesure de 15V, 15VS, 5VS, 3V3S, +15VSP, +12VSP.

Si mesure OK alors LED correspondante = verte.

Si mesure HS alors LED correspondante = rouge.


## 5. Test PILOTAGE (1/2)

On est déjà alimenté en 24V seul, suite au test précédent.

Test +15VSP => Test 1 : on doit avoir la tension sinon défaut Test 1.


## 6. Test des alims sous secouru (application du 12V)

Alimenter la UUT en 12V seul (appliquer 12V d’abord puis couper 24V).

Mesure 15VS, 5VS, 3V3S.

Si mesure OK alors LED correspondante = verte.

Si mesure HS alors LED correspondante = rouge.


## 7. Test PILOTAGE (2/2)

On est déjà alimenté en 12V seul suite au test précédent.

Attendre que le BP coupe le +15VSP. (6500ms pour être tranquille en mode test sur la version 092-31)

Test +15VSP => Test 2 : on doit avoir 0V flottant sinon défaut Test 2.

On affiche la LED « P » verte si Test 1 et Test 2 OK.

On affiche LED « P » rouge si Test 1 ou Test 2 HS.


## 8. Test FORCAGE

Test +15VSP => Test 1 : on doit avoir 0V flottant sinon défaut Test 1.

Faire clignoter la LED forçage « F » en rouge (c’est le signe pour l’opérateur qu’il doit « enlever » le strap de forçage alim +15VSP).

Attendre indéfiniment que l’entrée palpeur utilisée comme image du strap de forçage alim +15VSP passe à « strap enlevé » (= ETOR ouverte).

Dès qu’on est en « strap enlevé », on arrête le clignotement de la LED de forçage « F ».

Test +15VSP => Test 2 : on doit avoir la tension sinon défaut Test 2.

On affiche la LED « F » verte si Test 1 et Test 2 OK.

On affiche LED « F » rouge si Test 1 ou Test 2 HS.


## 9. Test alimentations alarme

Alimenter la UUT en 24V seul (appliquer 24V d’abord puis couper 12V).

Mesure des alims Détecteur Présence 1, Détecteur Présence 2, Sirène d’Intérieur, Sirène d’Extérieur.

Si mesure OK alors LED correspondante = verte.

Si mesure HS alors LED correspondante = rouge.


## 10. Test alimentation bus BA

Mesure de l’alim Bus BA.

Si mesure OK alors LED correspondante = verte.

Si mesure HS alors LED correspondante = rouge.


## 11. Test communication bus BA

Attente en permanence d’une trame BA PDV (le BP lance le scénario « Personnalisé » pour ce test) et on répond à chacune de ces trames (le résultat est analysé par le BP lui même et affiché sur l’IHM).


## 12. Redémarrage

Le soft de test s’arrête lorsqu’on retire la carte et donc que le palpeur n’est plus enfoncé. Cela force le uC en reset.

Donc l’insertion d’une nouvelle carte relancera automatiquement la procédure à zéro.

## Annexe. Mesures analogiques

Tableau manuscrit, non transcrit : voir `../etude/mesures-analogiques-annexe-spec-v4.pdf`.
