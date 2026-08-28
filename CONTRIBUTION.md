# Guide de contribution

## Vue d'ensemble

Ce guide décrit le processus à suivre pour contribuer au projet, ainsi que les conventions de nommage recommandées pour les branches et les messages de commit.

Toute forme de contribution est la bienvenue — pas seulement le code. Documentation, rapports de bugs, suggestions d'amélioration ou retours d'expérience sont tout aussi précieux pour le projet.

## Processus de contribution

Pour contribuer, vous commencez par forker le dépôt, ce qui vous donne votre propre copie du code. Vous clonez ensuite ce fork en local, effectuez vos modifications, puis les poussez vers votre propre dépôt.

Une fois vos modifications prêtes, vous ouvrez une Pull Request afin de proposer la fusion de votre travail dans la branche principale du dépôt d'origine. Chaque Pull Request fait l'objet d'une revue avant d'être fusionnée.

## Bonne pratique : isoler son travail dans des branches dédiées

Il est recommandé de ne pas travailler directement sur la branche principale de votre propre fork. Chaque fonctionnalité ou correction devrait être développée dans une branche dédiée. Poussez ensuite cette branche vers votre fork et ouvrez la Pull Request depuis cette branche vers la branche principale du dépôt d'origine. Cette approche facilite la synchronisation avec le dépôt d'origine et rend chaque contribution plus simple à isoler et à relire.

## Convention de nommage des branches

Il est recommandé de préfixer le nom de chaque branche selon la nature du changement qu'elle contient, afin d'en indiquer clairement le contexte. Par exemple : `feat/ajouter-om` pour l'ajout du provider Orange Money.

Préfixes courants :

- `feat/` — nouvelle fonctionnalité
- `fix/` — correction de bug
- `chore/` — maintenance, tooling
- `docs/` — documentation

Pour une référence plus formelle sur les règles de format et les types recommandés, vous pouvez consulter à titre indicatif la spécification [Conventional Branch](https://conventionalbranch.org/).

## Convention de nommage des messages de commit

Il est recommandé que chaque message de commit commence par un type indiquant la nature du changement, suivi éventuellement d'un scope entre parenthèses précisant la partie du code concernée. Par exemple : `feat(om): ajouter la récupération du token`.

Types courants :

- `feat` — nouvelle fonctionnalité
- `fix` — correction de bug
- `docs` — documentation
- `chore` — maintenance, tooling
- `refactor` — restructuration sans changement de comportement
- `test` — ajout ou modification de tests

Le scope entre parenthèses (`om`, `momo`, `requests`, etc.) vous permet d'identifier immédiatement la partie du SDK concernée par le changement.

Pour une référence complète (structure du corps du message, gestion des breaking changes, footers), vous pouvez vous appuyer sur la spécification [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/), qui fait autorité en la matière.

---

Merci pour l'intérêt que vous portez à ce projet — votre contribution est la bienvenue et grandement appréciée.
