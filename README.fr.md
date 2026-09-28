# Japanese AI Coach — banque d'exercices

*[English version](README.md)*

**Des exercices de japonais relus, construits sur de vraies phrases** : particules, conjugaisons et questions type JLPT (漢字読み, 表記, 文脈規定, 文法形式, 並べ替え ★), chacun avec sa phrase, sa réponse, ses traductions en français et en anglais, et son explication. C'est la banque partagée de [Japanese AI Coach](https://github.com/Aymeric-Dcn/Japanese-AI-Coach) : chaque copie de l'app la télécharge au démarrage, et fonctionne donc sans LLM local.

Contenu actuel : voir **[STATS.md](STATS.md)** (régénéré à chaque publication).

## Pourquoi une banque à part

Un modèle local sait générer des exercices, mais on ne peut pas lui confier leur vérification : lors de nos tests, Qwen 3 14B a accepté toutes les questions JLPT qu'on lui a fait vérifier, y compris des questions où deux réponses étaient justes. Un exercice n'entre donc dans cette banque qu'après trois filtres :

1. **Construit par des règles, pas par le modèle.** Les phrases viennent de [Tatoeba](https://tatoeba.org) ; le trou, la réponse et les lectures viennent d'un analyseur morphologique. Les mauvais choix sont construits comme au vrai test (autre lecture du kanji, voyelle longue ↔ courte, son voisé, kanji de même lecture qui ne forment pas un mot).
2. **Règles d'ambiguïté.** Les paires de particules souvent toutes deux justes (は/が, に/へ, と/や…) ne sont jamais proposées ensemble, les exercices は/が ne sont gardés que quand la grammaire tranche, le 並べ替え ne garde que des morceaux dont l'ordre est imposé, les conjugaisons qui conviendraient aussi sont écartées…
3. **Relecture.** Les lots sont relus par Claude (Anthropic) ; les exercices rejetés sont listés dans `rejected.jsonl` avec la raison, et les apps les retirent aussi.

Les signalements des utilisateurs (« ⚑ Signaler une erreur » dans l'app) alimentent la relecture suivante.

## Organisation

```
manifest.json            version, date, nombre, une entrée par fichier (chemin, sha256, nombre, thème)
STATS.md                 ce que contient la banque, par thème
exercises/<thème>.jsonl  un exercice relu par ligne
rejected.jsonl           clés retirées après relecture, avec la raison
inbox/                   contributions en attente de relecture (non relues)
```

Le format d'une ligne est décrit dans le [README anglais](README.md#layout). La `key` est stable : un même exercice n'est jamais ajouté deux fois.

## Utilisation

Dans l'app, rien à faire : la banque est téléchargée au démarrage (Progrès → Banque partagée → « Synchroniser maintenant » pour forcer). En ligne de commande : `python bank_sync.py pull`. Tes réponses, ta progression et tes données Anki ne sont jamais envoyées.

## Contribuer

Les apps qui ont un modèle local génèrent des exercices ; « Envoyer mes exercices à relire » les dépose dans `inbox/` (avec un jeton GitHub autorisé à écrire dans ce dépôt), ou `python bank_sync.py contribute --dest <dossier>` écrit un fichier à envoyer. Le mainteneur importe la boîte d'envoi, la fait relire, et publie ce qui passe.

## Sources et licence

La banque est publiée sous **[CC BY-SA 4.0](LICENSE.md)**. Elle est construite à partir de :

- **Tatoeba** : phrases et traductions — [CC BY 2.0 FR](https://creativecommons.org/licenses/by/2.0/fr/) ; chaque exercice renvoie vers sa phrase et ses auteurs sur tatoeba.org (`source_url`).
- **Niveaux JLPT des mots** : [open-anki-jlpt-decks](https://github.com/jamsinclair/open-anki-jlpt-decks) (MIT), d'après les listes de Jonathan Waller sur [tanos.co.uk](http://www.tanos.co.uk/jlpt/) (CC BY).
- **Lectures des kanji et sens courts** : [Full Japanese Study Deck](https://github.com/Ronokof/Full-Japanese-Study-Deck) (CC BY-SA 4.0). Ses parties non commerciales (données jpdb.io, audio) ne sont pas utilisées.
- **Lectures et découpage des mots** : [SudachiPy](https://github.com/WorksApplications/SudachiPy) et son dictionnaire (Apache 2.0).
- Explications rédigées par Qwen 3 (Apache 2.0) et Claude, relues.
