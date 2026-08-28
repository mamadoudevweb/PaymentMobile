# Architecture — Guide de conception

## 1. Vue d'ensemble

Le SDK expose une classe cliente unique (`Client`) comme point d'entrée public. Chaque provider de mobile money (Orange Money, MTN Momo, etc.) possède une API différente ; le SDK les unifie derrière une interface commune, sans dupliquer la logique d'authentification, de gestion des tokens et de communication HTTP.

## 2. Structure des packages

```
src/
    client.py        # Point d'entrée public : classe Client
    providers/        # Un sous-package par provider (om/, momo/, ...)
    requests/          # Interface HTTP commune à tous les providers
    shared/            # Code transverse (constantes, types, exceptions communes)
    tests/              # Tests unitaires et d'intégration (pytest)
.env               # Clés API, client secrets, configuration
```

### Rôle de chaque package

- **`client.py`** — Classe principale du SDK. Expose les méthodes de haut niveau (ex. envoyer une requête de paiement) et orchestre les providers.
- **`providers/`** — Un sous-package par provider. Chaque provider construit ses requêtes (payload, endpoints, authentification spécifique) et sérialise ses réponses, en s'appuyant sur `requests/`.
- **`requests/`** — Couche d'abstraction HTTP : authentification, récupération/rafraîchissement des tokens, envoi des requêtes GET/POST. Offre la même interface à tous les providers, quelle que soit leur API sous-jacente.
- **`shared/`** — Dépendances/utilitaires communs à plusieurs packages, tant que le volume reste faible (sinon, promouvoir en sous-package dédié).
- **`tests/`** — Tests avec `pytest`, organisés en unitaires et d'intégration.

## 3. Règle de dépendance

```
client → providers → requests
```

- `providers/` dépend de `requests/`.
- `requests/` ne dépend **jamais** de `providers/`.

Cette règle unidirectionnelle évite les dépendances circulaires et permet de tester chaque package indépendamment. Elle n'est pas figée : si une meilleure approche émerge en cours de route, la structure peut évoluer.

## 4. Gestion des erreurs API

Décision : **lever des exceptions plutôt que sérialiser les erreurs comme des réponses normales.**

Exemple : une requête mal formée renvoyant un `403` doit lever une exception dédiée (ex. `BadRequestError`) plutôt que d'être sérialisée comme une réponse classique.

Justification : sérialiser systématiquement oblige le code appelant à empiler des `if/elif` pour distinguer succès et erreurs. Les exceptions rendent le flux de contrôle explicite et évitent cette accumulation de conditions.

Recommandation pour la suite : définir une petite hiérarchie d'exceptions dans `shared/` (ex. `SDKError` en base, puis `BadRequestError`, `AuthenticationError`, etc.), mappées depuis les codes de statut HTTP au niveau de `requests/`.

## 5. Tests

- Framework : `pytest` (plutôt que `unittest`).
- Package dédié `tests/` avec séparation tests unitaires / tests d'intégration.
- L'isolation `requests/` ↔ `providers/` permet de mocker la couche HTTP pour tester chaque provider sans appel réseau réel.

## 6. Ordre d'implémentation

1. Orange Money (OM)
2. MTN Mobile Money (Momo)
3. Providers suivants, en suivant le même patron établi par OM

## 7. Configuration

- Fichier `.env` pour les secrets (clés API, client secret) — jamais commité.
- Chargement centralisé, probablement exposé via `shared/` ou directement dans `client.py`.

## Points ouverts / à trancher plus tard

- Hiérarchie exacte des exceptions (nombre de classes, granularité par provider ou générique).
- Convention de nommage interne des méthodes du `Client` (ex. `pay()`, `send_payment_request()`).
- Mécanisme de rafraîchissement automatique des tokens (à la demande vs proactif).
