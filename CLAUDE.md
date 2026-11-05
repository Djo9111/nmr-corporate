# NMR Corporate

Plateforme de suivi de la performance commerciale du réseau Corporate du
Crédit du Sénégal (CDS, Groupe Attijariwafa bank). Deux dépôts git distincts,
côte à côte, à lancer depuis ce dossier parent pour qu'une même tâche puisse
toucher les deux.

- `nmr-corporate-api/` : FastAPI, PostgreSQL, Alembic, Docker Compose
- `nmr-corporate-web/` : React 18, Vite, Tailwind v4

Ce dossier parent est un troisième dépôt git, sans remote, qui ne porte que le
contexte partagé : ce `CLAUDE.md` et le `.gitignore`. Il ignore les deux
dépôts applicatifs, qui restent indépendants et se commitent chacun
séparément. Un changement qui touche l'API et le front donne deux commits
avec le même intitulé. Une modification de ce fichier se commite ici, à part.

## Métier

Le réseau Corporate et le réseau retail sont deux réseaux distincts. Ils ne se
mélangent jamais, ni dans les données, ni dans les écrans, ni dans les
libellés.

Vocabulaire à employer tel quel, c'est celui des utilisateurs :

- FDC : numéro unique d'un gestionnaire
- arrêté : date de situation d'une extraction de production
- flux : production de la période, cumulable dans l'année
- stock ou encours : situation à une date, non cumulable
- TRO : taux de réalisation de l'objectif
- Var : variation par rapport au dernier arrêté de décembre de l'année
  précédente
- centre d'affaires : maille intermédiaire entre le gestionnaire et le réseau

Tous les montants sont en millions de FCFA, partout, sans exception. Jamais en
unités, jamais en milliers.

La hiérarchie des rôles : GESTIONNAIRE, RESPONSABLE_CA, DIRECTION, DG,
CHARGEUR, ADMIN. Les quatre premiers forment le périmètre d'activité, le
chargeur ne fait que charger des fichiers, l'admin administre.

## Règles permanentes

Elles ont été posées une fois et valent pour tout le projet.

1. Pas de bibliothèque de composants. Pas de shadcn, pas de MUI, pas de
   Recharts. Tout est écrit à la main dans `src/components/ui/` pour garder la
   main sur le projet.
2. Pas de `localStorage` ni de `sessionStorage`, pour éviter les failles JS.
   Le JWT vit dans un cookie httpOnly, avec un jeton CSRF en double-submit. Le
   serveur CDS sert en HTTP simple.
3. Les tests sont automatisés avec pytest. Pas de vérification manuelle au
   curl ou via Swagger.
4. Les écrans reproduisent la disposition des états papier que les
   utilisateurs connaissent déjà, pour qu'ils ne soient pas désorientés.
   L'interface reste sobre et professionnelle.
5. Diagnostiquer la cause racine avant de corriger. Un symptôme corrigé sans
   cause comprise revient.
6. Valider un composant à la fois. Ne pas enchaîner cinq modules avant le
   premier test.

## Conventions d'écriture

Les textes d'interface sont en français avec les accents. Les commentaires de
code sont en français sans accents, pour ne dépendre d'aucun encodage. Les
noms de variables, fonctions et fichiers sont en français sans accents aussi
(`perimetre`, `arrete`, `noeud`, `libelle`).

Un commentaire explique pourquoi, jamais quoi. Les commentaires utiles du
projet documentent un piège : un index unique partiel plutôt qu'une
contrainte, un ordre de déclaration de routes, une suppression avant
insertion. Garder ce style.

Pas de tiret long, pas de barre verticale dans les documents produits.

## Comment travailler ici

Une tâche à la fois, API puis front, avec les tests entre les deux. Livrer des
fichiers complets, jamais des fragments à recoller.

Une tâche qui touche l'API et le front se livre en un seul passage.
Exception : quand la forme de l'API est elle-même une décision de
conception, s'arrêter après l'API et rendre la main. Dans tous les cas,
énoncer le contrat des routes en une ligne avant d'écrire le front, dans
le même tour.

Avant de déclarer une chose faite : `pytest` côté API, `npm run lint` côté web.
Les deux doivent être propres.
