# Formation Jev + LangChain — NOVA, assistant de support

Le scénario : **Atelier Nova**, une boutique fictive d'accessoires de bureau, veut
aider son équipe à trier les tickets et préparer des réponses fiables.

**NOVA est un assistant de support de niveau 1.** Il analyse le ticket avec Jev,
consulte les commandes et procédures disponibles, puis prépare un dossier :
résumé, faits vérifiés, prochaine action et brouillon de réponse. Il recommande
une revue humaine si nécessaire. Il n'envoie rien et ne modifie aucune commande.

## Architecture

[Ouvrir le fichier architecture_nova.excalidraw](architecture_nova.excalidraw)
dans Excalidraw. Le schéma est éditable : rôle de NOVA, analyse Jev, règles Python,
boucle LangChain, outils, données fictives et intervention de l'équipe support.

## Les deux supports

- **tutoriel_jev.ipynb** : formation progressive avec explications, 49 cellules,
  exercices, projet final, quiz et corrigés.
- **agent.py** : application Python complète, commentée par sections, avec les
  mêmes données, rôle et fonctions métier que le notebook.

Le fichier `.env` contient les clés. `requirements.txt` liste les dépendances.
Le notebook et le script sont autonomes l'un par rapport à l'autre : leurs
modifications ne se synchronisent pas automatiquement.

## Ouvrir le notebook

Depuis ce dossier, dans PowerShell :

```powershell
.\.venv\Scripts\python.exe -m jupyterlab tutoriel_jev.ipynb
```

Dans VS Code, ouvrir le notebook et choisir le Python du dossier `.venv` comme
noyau. Exécuter les cellules dans l'ordre avec **Maj + Entrée**.

JupyterLab est déjà installé dans l'environnement local. Pour préparer le projet
sur un autre ordinateur avec Python 3.11 ou plus récent :

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

## Lancer le script Python

Renseigner `TYPESAFE_API_KEY` et `OPENAI_API_KEY` dans `.env`.
Les modèles sont configurables via `JEV_MODEL` et `OPENAI_MODEL`.
Les valeurs par défaut sont `jev-latest` et `gpt-4.1-mini`.

```powershell
# Un ticket complet
.\.venv\Scripts\python.exe agent.py --message "Où est ma commande AN-1001 ?"

# Jev uniquement, sans clé OpenAI
.\.venv\Scripts\python.exe agent.py --analyse --message "J'ai été débité deux fois."

# Conversation
.\.venv\Scripts\python.exe agent.py
```

En conversation : `/nouveau` change de client ; `/quitter` termine.
L'historique reste uniquement en mémoire. Le notebook propose une saisie masquée
si les clés ne sont pas dans `.env` ; le script demande de les configurer dans ce fichier.

## Guide du formateur

**Public :** débutants en agents IA connaissant les fonctions et dictionnaires Python.
**Format provisoire :** atelier de 3 heures. Le notebook contient des approfondissements
pour développer le contenu en journée de formation ou en série de vidéos.

| Module | Durée | Objectif observable |
| --- | ---: | --- |
| Scénario et rôle | 10 min | Définir mission, utilisateur, données et limites |
| Environnement | 15 min | Préparer les connexions sans afficher les clés |
| Première décision Jev | 20 min | Classer un ticket avec Choice |
| Trois primitives | 25 min | Distinguer catégorie, probabilité et score |
| Règles métier | 15 min | Expliquer et tester les seuils |
| Outils et agent | 25 min | Consulter une commande et produire un dossier |
| Conversation | 20 min | Réutiliser le contexte du même client |
| Évaluation | 20 min | Lire les erreurs sur 8 tickets annotés |
| Exploitation | 20 min | Observer les appels et utiliser le script |
| Projet et quiz | 10 min | Vérifier les acquis et préparer la suite |

### Préparation de la séance

1. Vérifier le noyau, les dépendances, les clés, les crédits et les accès aux modèles.
2. Tester un ticket réel sur les API avant la séance ; la vérification livrée utilise
   des réponses simulées et ne valide pas ces accès.
3. Lire les commandes et procédures fictives pour connaître les faits de référence.
4. Garder le lot d'évaluation et les traces facultatives désactivés au départ.
5. Prévoir que chaque nouvelle exécution d'une cellule de modèle génère des appels.
   Le parcours principal fait quatre requêtes Jev et deux traitements par l'agent ;
   chaque traitement peut appeler plusieurs fois le modèle de conversation.
6. Ne pas montrer les clés en projection. Utiliser uniquement les tickets fictifs.

### Animation

- **Avant chaque appel**, faire prédire aux participants le résultat attendu.
- **Après chaque appel**, distinguer le résultat de Jev, la règle Python et la rédaction.
- **Devant un désaccord**, examiner les critères et l'annotation avant de modifier le seuil.
- **Pendant le journal d'outils**, vérifier que la commande a réellement été consultée.
- **À la fin**, demander ce qui manque pour un service réel : droits d'accès,
  données à jour, surveillance, tests représentatifs et contrôle des actions.

### Exercices et validation

Le notebook fournit les consignes et corrigés :
- comprendre la catégorie « autre » et l'incertitude ;
- tester une référence de commande inconnue ;
- inspecter les messages et poursuivre un ticket ;
- mesurer l'exactitude du service et les urgences manquées ;
- ajouter le service « retour », sa procédure et trois exemples ;
- répondre à un quiz de huit questions.

Pour le projet final, évaluer : cohérence des catégories et procédures, usage des
outils, fidélité aux faits, traitement des cas incertains et explication des erreurs.
L'objectif n'est pas d'obtenir artificiellement 100 % sur quelques exemples.

## Vérifications et limites

Les 49 cellules sont valides. Le notebook a été exécuté avec API simulées, lot
d'évaluation activé, consultation d'outils et conversation. Les neuf fonctions
communes au notebook et au script ont été comparées. Les modes du script,
les seuils et l'arrêt avant le modèle de conversation en cas d'erreur Jev ont
également été contrôlés.

Ces contrôles vérifient le programme, pas la qualité des modèles ni l'accès aux comptes.
Les résultats réels doivent être évalués avec les clés des participants.
Le notebook est livré sans sorties enregistrées.

Les seuils, commandes et procédures sont pédagogiques. La revue humaine est une
recommandation dans le dossier, pas un transfert réellement exécuté.
Les données envoyées à Jev et au modèle de conversation sortent du poste local ;
les traces LangSmith sont facultatives.

## Documentation officielle

- [Jev et LangChain](https://docs.langchain.com/oss/python/integrations/providers/typesafe)
- [Primitives Jev : Choice](https://docs.typesafe.ai/primitives/choice),
  [Noul](https://docs.typesafe.ai/primitives/noul),
  [Score](https://docs.typesafe.ai/primitives/score)
- [Confiance](https://docs.typesafe.ai/confidence)
- [Agents LangChain](https://docs.langchain.com/oss/python/langchain/agents)
- [Traces LangSmith](https://docs.langchain.com/langsmith/trace-with-langchain)
