# Aviator Lens — plan des connecteurs de données

## État actuel

Aviator Lens est une application statique GitHub Pages. Elle peut analyser des multiplicateurs importés et les conserver localement dans le navigateur.

Aucune connexion automatique à 1xBet n'est activée à ce stade. Ne pas afficher de badge « connecté » ni inventer de données en direct.

## Jeux prévus

- `aviator` : Aviator
- `crash` : Crash
- `apple-of-fortune` : Apple of Fortune

Chaque jeu doit avoir un historique isolé, un statut de connexion indépendant et une source clairement identifiée.

## Sources et autorisations

### Aviator / Crash

La documentation Aviator Studio décrit une intégration destinée aux opérateurs partenaires. Elle exige notamment un Provider ID, une clé publique et des jetons de fournisseur. Son API d'historique des manches utilise les en-têtes `x-provider-token` et `x-provider-id`.

Références :
- https://docs.crash.aviator.studio/game-integration/launch-game/
- https://docs.crash.aviator.studio/round-history/
- https://docs.crash.aviator.studio/game-integration/integration-endpoints/

Ces identifiants ne sont pas des identifiants de joueur 1xBet. Ne pas demander à l'utilisateur de coller des cookies, mots de passe, jetons de session ou clés privées dans le site.

### Apple of Fortune

Aucune API publique officielle de lecture des résultats n'a été identifiée dans les recherches effectuées. Une intégration ne pourra être activée qu'après obtention d'une documentation et d'une autorisation adaptées.

## Architecture à implémenter après obtention d'une source autorisée

1. Un adaptateur indépendant par jeu, tous normalisant les résultats vers :
   `{ game, roundId, multiplier, outcome, timestamp, source, verified }`.
2. Un backend sécurisé (par exemple une fonction Cloudflare Worker) pour conserver les secrets côté serveur. GitHub Pages ne doit jamais contenir de clé privée ou de jeton partenaire.
3. Une validation stricte des données, déduplication par identifiant de manche et journal des erreurs.
4. Un stockage séparé par jeu.
5. Un état explicite : `non configuré`, `connexion en attente`, `connecté`, `erreur`.
6. Un mode import manuel en secours, toujours étiqueté comme tel.

## Règles de fiabilité

- N'utiliser que des endpoints documentés et autorisés.
- Ne pas scraper les endpoints internes du site, contourner les protections, ni réutiliser une session personnelle.
- Ne jamais présenter des statistiques historiques comme une prédiction fiable du prochain résultat.
- Pour Apple of Fortune, ne pas déduire une prochaine position gagnante à partir d'anciennes manches.
- Tant qu'aucune source autorisée n'est configurée, afficher clairement « connexion automatique non disponible ».

## Prochaine condition de mise en service

Obtenir auprès de 1xBet / du fournisseur de chaque jeu une confirmation écrite et la documentation d'accès aux résultats de manches pour un outil d'analyse externe. Pour Aviator Studio, demander si un accès en lecture seule à l'historique des manches peut être accordé à un développeur indépendant qui n'exploite pas le jeu comme opérateur.
