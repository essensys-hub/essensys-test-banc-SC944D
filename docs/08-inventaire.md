# 8. Inventaire, manques et points à vérifier

## Inventaire

103 fichiers d'origine, tous conservés. La table [`correspondance.csv`](correspondance.csv) donne pour chacun l'ancien chemin et le nouveau.

| Dossier | Fichiers | Contenu |
|---|---|---|
| `carte-interface-SI841A/schema/` | 2 | Schéma v4 (référence) et v1 |
| `carte-interface-SI841A/gerber-non-route-envoye-arf-test/` | 13 | Gerber et perçages avant routage |
| `carte-interface-SI841A/face-avant/` | 21 | Dessin de face avant v1 à v5, plan mécanique |
| `carte-interface-SI841A/serigraphie/` | 4 | Sérigraphies Illustrator |
| `points-de-test-SC944/` | 4 | Plans des points de test |
| `logiciel/` | 5 | Spec du logiciel de test v0 à v4 (+ transcription Markdown de la v4) |
| `mecanique-arf-test-6281/` | 24 | Dossier d'étude ARF Test |
| `etude/` | 10 | Croquis, scans, calculs, câbles |
| `datasheets/` | 9 | Composants, routeur, compteur |
| `photos/` | 11 | 3 photos de 2016, 8 photos de 2026 |

Fichiers ajoutés : `README.md`, `docs/`, `logiciel/spec-soft-test-bp-v4.md`, `.gitignore`.

## Modifications apportées aux fichiers d'origine

- **Renommage et déplacement** de tous les fichiers, sauf le contenu de `mecanique-arf-test-6281/` et les Gerber, qui gardent leur nom.
- **Photos** : les coordonnées GPS ont été retirées des métadonnées. La date et l'appareil sont conservés. L'extension `.jpeg` des photos de 2026 est devenue `.jpg`.
- Rien d'autre n'a été modifié ni supprimé. Les fichiers `.DS_Store` ont été retirés.

## Doublons

| Fichiers | Constat |
|---|---|
| `face-avant/ci-v5.ai` et `face-avant/archives/ci-v4.ai` | Identiques octet pour octet |
| Les six DXF `ci-v4_*.dxf` | Contenus différents (options d'export), tous conservés |

## Ce qui est ailleurs

Le dépôt public `essensys-board-SC841A` contient ce qui manque à ces archives : le projet Altium de la SI841A, ses Gerber routés, son implantation, sa nomenclature, le code source et le binaire du logiciel de test, et ses notes de version. Rien n'y est recopié ici.

## Ce qui manque

| Manque | Conséquence |
|---|---|
| Notice d'utilisation du banc | Le mode opératoire est déduit de la spec et du code |
| Plan de câblage entre le lit de clous et la SI841A | La correspondance clou → connecteur n'est pas documentée |
| Scans `EPSON005` et `EPSON006` | La numérotation saute de 004 à 007 |

## Points à vérifier sur le banc

- [ ] Le détecteur d'ouverture coupe bien la phase et le neutre du 230 V.
- [ ] État des trois alimentations TDK-Lambda et des clous.
- [ ] Type de liaison entre le lit de clous et la SI841A (nappe HE10 vue sur les photos).
- [ ] Le logiciel chargé dans le banc est-il bien `Banc_BP_V2_FAST` ? Le relire et le comparer au `.hex` de `essensys-board-SC841A` (la protection en lecture est désactivée dans le code).
- [ ] Compatibilité des positions de clous avec une SC944 indice D.
- [ ] Une des trois photos de 2016 a une fin de fichier abîmée (avertissement « premature end of data segment ») ; elle s'affiche tout de même.

## Remarque sur l'analyse

Les archives de ce dépôt ne contiennent que des documents. La première version de cette documentation (11/11/2026) affirmait à tort que le logiciel et les fichiers de conception étaient perdus et que la SI841A n'était pas la SC841A : le dépôt `essensys-board-SC841A` n'avait pas été consulté. La correction du même jour s'appuie sur la lecture de son code source.
