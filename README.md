

# KOORI

### Agent IA vocal de recouvrement éthique et de restructuration de crédits — Banques & Microfinances de la zone UEMOA

> *Récupérer les créances en souffrance sans humilier personne, à l'échelle de milliers de dossiers, dans la langue du client.*

---

## Le problème

Les banques de l'UEMOA détiennent plus de **3 442 milliards FCFA de créances en souffrance** — soit un taux brut de dégradation d'environ **9 %** (source : BCEAO, 2026). Autrement dit, près d'**un crédit sur onze** n'est pas remboursé.

Aujourd'hui, le recouvrement amiable est **entièrement manuel** :

- des agents appellent les débiteurs **un par un** (30 à 50 dossiers par jour) ;
- les relances sont rédigées dans un **français administratif** que beaucoup de clients ne comprennent pas ;
- les méthodes sont souvent **agressives**, ce qui détruit la confiance et le lien client ;
- résultat : les banques recouvrent **tard**, recouvrent **peu**, et **sur-provisionnent**.

Ce problème est vécu comme « normal ». Il ne l'est pas.

---

## La solution

**KOORI** est une plateforme SaaS qui remplace le recouvrement manuel par un **agent IA négociateur**, poli et multilingue, capable de traiter des milliers de dossiers en parallèle.

L'agent fait trois choses :

**1. Il analyse et segmente** chaque dossier en souffrance grâce au Machine Learning : *oubli*, *difficulté temporaire*, ou *mauvaise foi probable*, et prédit la meilleure stratégie de contact (canal, moment, ton, langue).

**2. Il contacte et négocie** par WhatsApp et par appel vocal, dans la langue du client (français d'abord, langues locales ensuite). Il propose un **plan de remboursement réaliste**, dans les limites fixées par la banque (durée maximale, décote maximale, respect du **taux d'usure UEMOA**), puis génère instantanément l'accord (échéancier PDF) et un **lien de paiement mobile money**.

**3. Il rend compte à la banque** via un tableau de bord temps réel : promesses de paiement, promesses tenues, projection de recouvrement du portefeuille.

---

## Pourquoi maintenant

- Le **stock de créances en souffrance augmente** alors que le crédit accélère (+25 % de volume mensuel de crédits en un an).
- Les **LLM vocaux multilingues** sont devenus utilisables (voix naturelle, faible latence) — c'était techniquement impossible il y a deux ans.
- La **preuve de marché existe** en Afrique anglophone (Bfree, 12 M$+ levés, 740 M$ de créances traitées) — mais **aucun équivalent n'existe en Afrique francophone**.

---

## Fonctionnalités

### MVP
- [ ] Scoring ML et segmentation du portefeuille de créances en souffrance
- [ ] Agent IA de négociation (chat) avec *tool calling* et limites définies par la banque
- [ ] Génération automatique du plan de remboursement (échéancier PDF + lien de paiement)
- [ ] RAG sur les règles réglementaires (taux d'usure, politique interne)
- [ ] Tableau de bord banque : promesses de paiement, projection de recouvrement

### Bonus (si le temps le permet)
- [ ] Canal vocal (conversation téléphonique réaliste, TTS/STT)
- [ ] Support d'une langue locale (dioula) pour la démo
- [ ] Agent superviseur validant chaque accord avant confirmation (multi-agents)
- [ ] Rappels automatiques SMS/WhatsApp avant chaque échéance

---

## Architecture

```
┌──────────────────────────────────────────────────────────────┐
│                      TABLEAU DE BORD BANQUE                    │
│              (React) — portefeuille, promesses, KPIs          │
└───────────────────────────┬──────────────────────────────────┘
                            │  REST API
┌───────────────────────────▼──────────────────────────────────┐
│                    BACKEND (FastAPI / Python)                 │
│                                                              │
│  ┌────────────────┐   ┌────────────────┐   ┌──────────────┐  │
│  │  Module ML     │   │  Agent LLM     │   │  Couche RAG  │  │
│  │  Scoring &     │──▶│  Négociation   │◀──│  FAISS +     │  │
│  │  segmentation  │   │  (tool calling)│   │  embeddings  │  │
│  │  (scikit-learn)│   │                │   │  (règles     │  │
│  └────────────────┘   └───────┬────────┘   │  BCEAO/usure)│  │
│                              │            └──────────────┘  │
│              ┌───────────────┼───────────────┐              │
│         ┌────▼────┐    ┌─────▼─────┐    ┌────▼─────┐        │
│         │ PDF     │    │ Lien      │    │ Canaux   │        │
│         │ échéance│    │ mobile    │    │ WhatsApp │        │
│         │         │    │ money     │    │ / Voix   │        │
│         └─────────┘    └───────────┘    └──────────┘        │
└──────────────────────────────────────────────────────────────┘
```

---

## Stack technique

| Domaine | Technologies |
|---|---|
| Langage | Python |
| Machine Learning | scikit-learn, pandas, NumPy |
| Agent & LLM | OpenAI / Claude / Groq API, *tool calling* |
| RAG | FAISS, vector embeddings |
| Backend | FastAPI |
| Frontend | React |
| Voix (bonus) | APIs Speech-to-Text / Text-to-Speech |
| Livrables | Génération PDF, liens de paiement (simulés) |
| Déploiement | Render / Vercel |

---

## Roadmap (4 semaines)

| Semaine | Objectif |
|---|---|
| **1** | Dataset synthétique (500-1000 dossiers) + module de scoring/segmentation ML |
| **2** | Agent de négociation LLM avec *tool calling* (lecture dossier, calcul échéancier, respect des limites) + couche RAG réglementaire |
| **3** | Canaux de conversation (chat WhatsApp + voix en démo) + génération PDF et lien de paiement |
| **4** | Tableau de bord React, tests des cas limites, garde-fous, déploiement, script de démo |

---

## Démo

Une **négociation en direct** entre l'agent IA et un « débiteur » joué par un membre du jury :

1. Affichage d'un portefeuille de 1000 dossiers en souffrance.
2. Le téléphone du « débiteur » sonne — l'agent le salue par son nom, comprend sa situation (« j'ai perdu mon emploi, je peux payer 25 000/mois »), négocie et conclut un accord.
3. En simultané, le tableau de bord de la banque se met à jour en temps réel : promesse enregistrée, échéancier généré, projection de recouvrement affichée.

---

## Validation marché

- **BCEAO (2026)** : 3 442 milliards FCFA de créances en souffrance dans l'UEMOA (taux brut 8,8–9,1 %).
- **Bfree (Nigeria)** : 12 M$+ levés, 740 M$ de créances traitées, 30+ institutions financières — **uniquement en zone anglophone**.
- **Opportunité** : aucun acteur ne couvre le français, les langues locales UEMOA, ni le cadre réglementaire BCEAO/OHADA.

---

## Éthique & données

- **Aucune donnée client réelle** n'est utilisée : le projet repose entièrement sur des **données synthétiques**.
- L'agent est conçu pour un recouvrement **respectueux et non menaçant** (à l'opposé des pratiques de harcèlement).
- Chaque interaction est **tracée** pour la conformité et les éventuels litiges.

---

## Auteur

Projet Capstone — Bootcamp GenAI & Machine Learning (Tech Talent Accelerator).

Développeur Full Stack spécialisé en Intelligence Artificielle.

---

## Licence

Projet académique / démonstration. Tous droits réservés à l'auteur.
