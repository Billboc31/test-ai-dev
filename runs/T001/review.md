# PR Review — T001 — Define technical architecture and technology stack

## Résumé

Ticket de documentation pure : sélectionner le stack technique (frontend, backend, base de données, déploiement) et mettre à jour la documentation existante. Aucun code source. L'implémentation couvre les 8 fichiers prévus par le plan approuvé, avec un ADR-002 complet et bien motivé.

## Vérifications effectuées

| Fichier | Contenu attendu | Vérifié |
|---|---|---|
| `.ai-dev-factory/project.yml` | `stack: unknown` → `react-ts-express-sqlite` | ✓ |
| `docs/architecture.md` | Stack concret, diagramme de structure mis à jour | ✓ |
| `docs/project-overview.md` | Table "Stack" avec les 6 couches | ✓ |
| `docs/dependencies.md` | Dépendances planifiées (frontend, backend, dev) | ✓ |
| `docs/deployment.md` | Modèle V1 (single process) + split triggers | ✓ |
| `docs/local-development.md` | Prérequis Node.js 20 LTS + URLs locales | ✓ |
| `docs/ai/global-context.md` | Ligne stack, suppression de `stack: unknown` des risques | ✓ |
| `docs/ai/decisions-log.md` | ADR-002 avec rationale par couche | ✓ |

**Périmètre** : aucun fichier source créé, aucun `package.json`, aucun code applicatif. ✓

## Points validés

- **Exhaustivité** : les 4 couches requises par le ticket (frontend, backend, base de données, déploiement) sont toutes adressées dans ADR-002.
- **Qualité des décisions** : chaque choix est justifié face aux alternatives crédibles (Next.js, FastAPI, NestJS, FullCalendar, PostgreSQL). Les rejets sont motivés, pas arbitraires.
- **Limites documentées** : SQLite — limites connues et trigger de migration vers PostgreSQL explicitement documenté. `react-big-calendar` — risque maintenance-mode signalé avec exit path. Les deux choix conservateurs sont assumés et bornés.
- **Cohérence interne** : les 8 fichiers sont cohérents entre eux (même stack, mêmes versions, mêmes noms de paquets).
- **Secrets / sécurité** : aucun secret introduit, aucune credential, aucune dépendance externe non justifiée. ✓
- **`project.yml`** : `stack: unknown` éliminé de la source de vérité centrale. ✓
- **URLs locales** (`localhost:3000` / `localhost:5173`) : cohérentes avec le modèle de déploiement V1 (Express + Vite dev server séparés). ✓

## Problèmes détectés

### Mineurs (non bloquants)

**1. Ordre "newest first" non respecté dans `decisions-log.md`**

L'en-tête du fichier stipule "newest first". ADR-002 (2026-06-19) apparaît *après* ADR-001 (2026-06-19, mais logiquement antérieur) — ordre non prioritaire pour deux entrées du même jour. Acceptable en l'état car les deux ADR portent la même date, mais si l'ordre est important pour de futurs lecteurs, ADR-002 devrait précéder ADR-001.

**2. Conséquence stale dans ADR-001**

ADR-001, section "Consequences", contient toujours : *"Technology stack remains to be chosen (see `stack: unknown`)"*. Ce texte est historiquement correct (ADR-001 précède ADR-002) mais pourrait dérouter un lecteur arrivant sur le fichier. Le Memory Updater pourra annoter ou réécrire cette ligne lors de la mise à jour mémoire post-approbation.

**3. Nuance workflow : Coder a modifié `docs/ai/`**

L'invariant global stipule "Only Memory Updater may modify `docs/ai/`; only after IMPLEMENTATION_APPROVED". Le Coder a mis à jour `docs/ai/global-context.md` et `docs/ai/decisions-log.md`. Pour ce ticket, cette déviation est justifiée :
- La mise à jour de `docs/ai/` *est* la livraison principale de T001 (définition du stack).
- Elle était explicitement dans le plan approuvé (PLAN_APPROVED sanction humaine).
- Aucun impact négatif sur l'intégrité des artefacts.

À noter pour les tickets futurs : l'invariant reste valide pour les tickets de code. Pour les tickets "documentation/architecture uniquement", la frontière Coder/Memory Updater mérite d'être clarifiée dans `docs/ai/workflow.md` (ticket distinct).

## Risques éventuels

- **`react-big-calendar` en maintenance-mode** : risque documenté et assumé. Exit path identifié (FullCalendar open-core). Acceptable pour V1.
- **SQLite pour déploiement local uniquement** : parfaitement approprié pour V1 single-user. Le trigger de migration PostgreSQL est clair.
- **`docs/ai/workflow.md` toujours absent** : mentionné dans Known Risks mais hors scope de T001. À adresser dans un ticket dédié.

## Décision

- **APPROVED**

L'implémentation est correcte, complète et conforme au ticket. Tous les choix techniques sont justifiés. Aucun code source n'a été créé. Les deux observations mineures (ordre ADR, texte stale ADR-001) ne nécessitent pas de correction avant merge — elles peuvent être adressées par le Memory Updater ou dans un ticket ultérieur.

## Actions demandées

Aucune correction bloquante.

Suggestions post-merge (non bloquantes) :
1. Memory Updater : annoter la conséquence stale d'ADR-001 ou la supprimer lors de la mise à jour mémoire.
2. Ticket distinct : créer `docs/ai/workflow.md` (identifié comme risque dans Known Risks).
3. Ticket distinct : clarifier dans le workflow si le Coder peut modifier `docs/ai/` pour les tickets "documentation/architecture pure".

IMPLEMENTATION_APPROVED
