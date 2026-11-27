# Beta-fusion-cahiermalin-x-cahierdecours
en collaboration avec bradley

**Cahier Malin × Notion** (version bêta) : la fusion de deux applis de prise de cours.

- [Cahier de cours](https://github.com/LB-HI/Cahier-de-cours-ft-notion) : éditeur riche (feuille Seyès) qui compile les cours dans Notion.
- [Cahier Malin](https://github.com/bradleyefrei/cahier-malin) : enregistrement du cours, rédaction par IA, kit de révision et planning du soir.

Les deux projets d'origine ne sont pas modifiés : cette fusion est une app à part.

## Ce que fait l'app

**Mes cours**
- Cours rangés par matière, avec leur état : brouillon, rédigé, révisions prêtes, contrôle dans X jours, envoyé dans Notion.
- Encadré « Ce soir » avec les séances de révision du jour.

**Écrire un cours** (éditeur de Cahier de cours)
- Titres T1 à T4 colorés, gras, italique, souligné, barré, code, liens, 9 couleurs de texte, surlignage et fond de paragraphe.
- Listes, cases à cocher, indentation, tableaux, 2 ou 3 colonnes, sections dépliantes, blocs de code, sommaire.
- Équations LaTeX (KaTeX) et palette de symboles.
- Encadrés : Correction, À retenir, Attention, Définition, Exemple, Méthode, Exercice, Astuce, Remarque.
- 🎤 **Dictée** : la voix est écrite directement dans la feuille. Elle remplace l'enregistrement micro de Cahier Malin.
- ✨ **Rédiger avec l'IA** : les notes brutes deviennent un cours structuré (résumé, à retenir, parties, définitions, formules, consignes du prof). Le texte d'origine est conservé.

**Réviser** (Cahier Malin)
- Fiches de synthèse, flashcards à 3 niveaux, quiz avec séries, moyens mnémotechniques, défis Feynman, carte mentale.
- Question libre sur le cours, réponse par l'IA.
- Lecture à voix haute du résumé et des fiches.
- Points d'expérience et série de jours.

**Planning du soir** (Cahier Malin)
- Séances de 25 min entre 21:00 et 23:00 jusqu'à la date du contrôle (répétition espacée J+1, J+3, J+6…).
- Export vers l'agenda (`.ics`).

**Notion**
- Envoi du cours à la suite de la page de la matière, dans une sous-page, ou dans une nouvelle matière.
- Option : ajouter les révisions (fiches, flashcards, quiz, méthodes) dans une section dépliante « 🧠 Révisions ».

## Différences avec Cahier Malin (version Windows)

| Cahier Malin | Fusion |
|---|---|
| Enregistrement + Whisper hors ligne | Dictée de claude.ai (Chrome, Edge ou app Claude pour ordinateur) |
| IA locale Ollama, ou clé API Claude | IA via le compte claude.ai de l'utilisateur, sans clé |
| Données dans `%LOCALAPPDATA%` et `Mes cours\` | Données sur le compte claude.ai (tous les appareils), sinon dans le navigateur |
| Rédaction automatique chaque soir à 18:00 | Rédaction lancée à la main |

## Fonctionnement technique

`index.html` est une page autonome (HTML, CSS et JavaScript sans framework). Elle est publiée comme artifact claude.ai et utilise ses capacités :

| Capacité | Usage |
|---|---|
| `mcp` (connecteur Notion) | `notion-update-page`, `notion-create-pages` |
| `sample` | Rédaction du cours, kit de révision, questions |
| `dictation` | Dictée |
| `db` + `user` | Sauvegarde des cours dans l'espace privé de l'utilisateur (`data/users/<id>/`) |
| `downloads` | Export du planning `.ics` |
| `desktop` | Installation comme application |

Ouverte hors de claude.ai, la page fonctionne comme éditeur et outil de révision, avec une sauvegarde dans le navigateur. L'IA, la dictée et Notion n'y sont pas disponibles.

### Données d'un cours

```json
{ "kind": "course", "title": "…", "subject": "Anglais", "subjectId": "<id page Notion>",
  "createdAt": "AAAA-MM-JJ", "status": "draft | ready", "examDate": "AAAA-MM-JJ | null",
  "html": "contenu de l'éditeur", "raw": "texte avant rédaction par l'IA",
  "notes": { "titre", "resume", "sections", "definitions", "a_retenir", "dates_formules", "consignes", "date_controle_mentionnee" },
  "kit": { "fiches", "flashcards", "quiz", "mnemos", "feynman", "carte_mentale" },
  "done": {}, "cards": {}, "quizBest": null, "notion": { "url", "at" } }
```

### Configuration

Les identifiants des pages Notion (page racine et matières) sont en haut du script, dans `ROOT` et `DEFAULTS`.
