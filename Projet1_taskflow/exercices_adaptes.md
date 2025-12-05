# Exercices TaskFlow - Progression par Concepts

## **📚 STRUCTURE DES EXERCICES**

Chaque exercice suit cette logique :
- **Objectif** : Compétence visée
- **⭐ Niveau** : Facile / Intermédiaire / Avancé
- **Consigne** : Énoncé clair
- **✅ Validation** : Critères de réussite
- **💡 Indice** : Aide progressive (style France IOI)

---

# **CONCEPT 1 : GIT & VERSIONING** 

## **Exercice 1.1 : Premier Commit** ⭐ FACILE
**Objectif** : Initialiser un repository et faire son premier commit  
 

**Consigne** :
1. Créez un dossier `taskflow-exercises`
2. Initialisez un repository Git
3. Créez un fichier `README.md` avec votre nom et la date
4. Faites votre premier commit avec le message "Initial commit"

**✅ Validation** :
```bash
# Vérifier l'historique
git log --oneline
# Doit afficher 1 commit
```

**💡 Indice** :
- Utilisez `git init`
- Utilisez `git add` puis `git commit -m "message"`

---

## **Exercice 1.2 : Branches de Développement** ⭐ FACILE
**Objectif** : Créer et naviguer entre les branches  
 

**Consigne** :
1. Créez une branche `develop`
2. Créez une branche `feature/authentication`
3. Basculez sur `feature/authentication`
4. Créez un fichier `auth.js` avec un commentaire "TODO: Authentication"
5. Commitez ce fichier
6. Retournez sur la branche `main`

**✅ Validation** :
```bash
git branch --list
# Doit afficher : main, develop, feature/authentication
git log --all --oneline --graph
```

**💡 Indice** :
- `git branch nom-branche` pour créer
- `git checkout nom-branche` pour basculer
- Ou `git checkout -b nom-branche` pour créer et basculer

---

## **Exercice 1.3 : Résolution de Conflits Simple** ⭐⭐ INTERMÉDIAIRE
**Objectif** : Gérer un conflit de fusion basique  
  

**Consigne** :
1. Sur la branche `main`, créez `config.js` avec :
   ```javascript
   const PORT = 3000;
   ```
2. Commitez
3. Créez et basculez sur une branche `feature/new-port`
4. Dans `config.js`, changez le port à 5000
5. Commitez
6. Retournez sur `main` et changez le port à 8080
7. Commitez
8. Tentez de merger `feature/new-port` dans `main`
9. Résolvez le conflit en gardant le port 5000
10. Finalisez le merge

**✅ Validation** :
- Le fichier `config.js` contient `PORT = 5000`
- L'historique montre un commit de merge
- Pas de marqueurs de conflit (`<<<<`, `====`, `>>>>`)

**💡 Indice** :
- Git vous alertera d'un conflit
- Éditez manuellement le fichier
- Utilisez `git add` puis `git commit` après résolution

---

## **Exercice 1.4 : Git Flow Simplifié** ⭐⭐ INTERMÉDIAIRE
**Objectif** : Appliquer le workflow Git Flow  
  

**Consigne** :
Créez la structure suivante :
```
main (production)
├── develop (développement)
    ├── feature/user-model (fonctionnalité 1)
    └── feature/task-model (fonctionnalité 2)
```

1. Créez les branches `develop`, `feature/user-model`, `feature/task-model`
2. Sur `feature/user-model`, créez `user.model.js`
3. Sur `feature/task-model`, créez `task.model.js`
4. Mergez les deux features dans `develop`
5. Mergez `develop` dans `main`

**✅ Validation** :
```bash
git log --all --graph --decorate --oneline
# Doit montrer la structure de branches
# main contient user.model.js et task.model.js
```

---

## **Exercice 1.5 : Commits Conventionnels** ⭐⭐ INTERMÉDIAIRE
**Objectif** : Maîtriser les messages de commit conventionnels  
  

**Consigne** :
Créez 7 commits en suivant la convention Conventional Commits :

1. **feat**: Ajout d'une nouvelle fonctionnalité d'authentification
2. **fix**: Correction d'un bug dans la validation des emails
3. **docs**: Mise à jour du README avec les instructions d'installation
4. **style**: Formatage du code (ajout de points-virgules)
5. **refactor**: Refactorisation de la fonction de hashage
6. **test**: Ajout de tests unitaires pour l'authentification
7. **chore**: Mise à jour des dépendances npm

**✅ Validation** :
```bash
git log --oneline
# Doit afficher les 7 commits avec le bon préfixe
```

**Format attendu** :
```
feat(auth): add user authentication
fix(validation): correct email validation regex
docs: update README with installation steps
```

---

## **Exercice 1.6 : Rebase Interactif** ⭐⭐⭐ AVANCÉ
**Objectif** : Réorganiser l'historique avec rebase  
  

**Consigne** :
1. Créez 5 commits désordonnés :
   - "add feature X"
   - "fix typo"
   - "wip"
   - "update feature X"
   - "forgot to add file"
2. Utilisez `git rebase -i HEAD~5` pour :
   - Fusionner les commits liés à la feature X
   - Supprimer le commit "wip"
   - Réordonner les commits logiquement
3. Résultat : 2 commits propres et descriptifs

**✅ Validation** :
```bash
git log --oneline
# Doit afficher seulement 2-3 commits propres
```

**💡 Indice** :
- `pick` : garder le commit
- `squash` : fusionner avec le précédent
- `reword` : modifier le message
- `drop` : supprimer le commit

---

## **Exercice 1.7 : Annulation et Récupération** ⭐⭐⭐ AVANCÉ
**Objectif** : Maîtriser reset, revert et reflog  
  

**Consigne** :
Scenario : Vous avez fait des erreurs et devez les corriger

1. Créez 3 commits avec des fichiers `a.txt`, `b.txt`, `c.txt`
2. Utilisez `git reset --hard HEAD~2` (erreur !)
3. Récupérez les commits perdus avec `reflog`
4. Créez un commit avec une erreur dans `d.txt`
5. Annulez ce commit avec `git revert` (pas reset !)

**✅ Validation** :
- Tous les fichiers a, b, c, d existent
- L'historique montre un commit de revert
- Vous avez documenté les commandes utilisées

**💡 Indice** :
- `git reflog` montre tout l'historique des actions
- `git reset` réécrit l'historique (dangereux)
- `git revert` crée un nouveau commit d'annulation (sûr)

---

## **Exercice 1.8 : Cherry-Pick Sélectif** ⭐⭐⭐ AVANCÉ
**Objectif** : Appliquer des commits spécifiques entre branches  
  

**Consigne** :
1. Sur `develop`, créez 5 commits :
   - Commit A : ajout fonction login
   - Commit B : ajout fonction register
   - Commit C : bug dans login
   - Commit D : ajout fonction logout
   - Commit E : bug dans register
2. Sur `main`, cherry-pick uniquement les commits A, B, et D
3. Créez une branche `hotfix` depuis `main`
4. Cherry-pick les commits C et E sur `hotfix`

**✅ Validation** :
- `main` contient login, register, logout (sans bugs)
- `hotfix` contient les corrections de bugs

**💡 Indice** :
```bash
git cherry-pick <commit-hash>
git log --all --graph --oneline
```

---

## **Exercice 1.9 : Stash et Workflow** ⭐⭐ INTERMÉDIAIRE
**Objectif** : Utiliser git stash pour gérer le travail en cours  
  

**Consigne** :
Scenario : Vous travaillez sur une feature mais devez faire un hotfix urgent

1. Sur `feature/new-ui`, modifiez `style.css` (ne commitez pas)
2. Urgence ! Vous devez corriger un bug sur `main`
3. Utilisez `git stash` pour sauvegarder votre travail
4. Basculez sur `main` et créez un hotfix
5. Retournez sur `feature/new-ui`
6. Récupérez votre travail avec `git stash pop`

**✅ Validation** :
```bash
git stash list # doit être vide
git status # doit montrer les modifications récupérées
```

---

## **Exercice 1.10 : Collaboration avec Pull Request** ⭐⭐⭐ AVANCÉ
**Objectif** : Simuler une collaboration d'équipe  
  

**Consigne** :
1. Créez un repository sur GitHub
2. Clonez-le localement
3. Créez une branche `feature/authentication`
4. Implémentez un fichier `auth.js` avec :
   ```javascript
   function login(email, password) {
     // TODO: implement
   }
   ```
5. Poussez la branche sur GitHub
6. Créez une Pull Request avec :
   - Titre descriptif
   - Description détaillée
   - Checklist des changements
7. Faites un deuxième commit pour corriger un commentaire
8. Mergez la PR (sans conflit)

**✅ Validation** :
- PR créée et mergée sur GitHub
- Historique propre et linéaire
- Description complète de la PR

---

# **CONCEPT 2 : BASE DE DONNÉES SQL**

## **Exercice 2.1 : Création de Tables Simples** ⭐ FACILE
**Objectif** : Créer les tables de base  
  

**Consigne** :
Créez une base de données `taskflow_db` avec les tables suivantes :

```sql
-- Table Users
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  email VARCHAR(255) UNIQUE NOT NULL,
  password VARCHAR(255) NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Table Tasks
CREATE TABLE tasks (
  id SERIAL PRIMARY KEY,
  user_id INTEGER REFERENCES users(id),
  title VARCHAR(255) NOT NULL,
  completed BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**✅ Validation** :
```sql
\dt -- PostgreSQL : lister les tables
DESCRIBE users; -- MySQL : décrire la structure
```

---

## **Exercice 2.2 : Insertion de Données** ⭐ FACILE
**Objectif** : Insérer des données de test  
 

**Consigne** :
Insérez les données suivantes :

1. 3 utilisateurs avec des emails différents
2. 5 tâches réparties entre les utilisateurs :
   - 2 tâches pour l'utilisateur 1
   - 2 tâches pour l'utilisateur 2
   - 1 tâche pour l'utilisateur 3

**✅ Validation** :
```sql
SELECT COUNT(*) FROM users; -- Doit retourner 3
SELECT COUNT(*) FROM tasks; -- Doit retourner 5
```

**💡 Indice** :
```sql
INSERT INTO users (email, password) 
VALUES ('user@example.com', 'hashed_password');
```

---

## **Exercice 2.3 : Requêtes SELECT de Base** ⭐ FACILE
**Objectif** : Maîtriser les requêtes de lecture simples  
  

**Consigne** :
Écrivez les requêtes SQL pour :

1. Récupérer toutes les tâches
2. Récupérer uniquement les tâches terminées
3. Récupérer les tâches de l'utilisateur avec l'email 'john@example.com'
4. Compter le nombre total de tâches
5. Compter le nombre de tâches terminées

**✅ Validation** :
Chaque requête doit retourner le bon résultat

**💡 Indice** :
```sql
SELECT * FROM tasks WHERE completed = TRUE;
SELECT COUNT(*) FROM tasks WHERE ...;
```

---

## **Exercice 2.4 : Jointures et Relations** ⭐⭐ INTERMÉDIAIRE
**Objectif** : Maîtriser les JOIN  
  

**Consigne** :
Écrivez les requêtes pour :

1. Afficher toutes les tâches avec le nom de leur propriétaire :
   ```
   task_title | user_email
   ```

2. Afficher tous les utilisateurs avec le nombre de tâches qu'ils ont :
   ```
   user_email | task_count
   ```

3. Afficher les utilisateurs qui ont au moins une tâche terminée

4. Afficher les tâches des utilisateurs créés dans les 7 derniers jours

**✅ Validation** :
```sql
-- La requête 2 doit utiliser COUNT et GROUP BY
-- La requête 3 doit utiliser INNER JOIN
-- La requête 4 doit utiliser une condition sur created_at
```

---

## **Exercice 2.5 : Mise à Jour et Suppression** ⭐⭐ INTERMÉDIAIRE
**Objectif** : Maîtriser UPDATE et DELETE en toute sécurité  
  

**Consigne** :
1. Marquez toutes les tâches de 'john@example.com' comme terminées
2. Supprimez toutes les tâches terminées de plus de 30 jours
3. Changez l'email d'un utilisateur spécifique
4. **ATTENTION** : Écrivez d'abord un SELECT avant chaque UPDATE/DELETE

**✅ Validation** :
```sql
-- Vérification avant UPDATE
SELECT * FROM tasks WHERE user_id = 1;
-- Puis UPDATE
UPDATE tasks SET completed = TRUE WHERE user_id = 1;
-- Vérification après
SELECT * FROM tasks WHERE user_id = 1;
```

**💡 Indice** :
Toujours tester avec `SELECT` avant `UPDATE` ou `DELETE` !

---

## **Exercice 2.6 : Contraintes et Validations** ⭐⭐ INTERMÉDIAIRE
**Objectif** : Ajouter des contraintes de base de données  
  

**Consigne** :
Modifiez les tables pour ajouter :

1. Une contrainte CHECK : le titre d'une tâche doit avoir au moins 3 caractères
2. Une contrainte CHECK : l'email doit contenir '@'
3. Un index sur `user_id` dans la table `tasks`
4. Un index sur `completed` et `created_at` (index composite)
5. Une valeur par défaut pour `updated_at`

```sql
ALTER TABLE tasks 
ADD CONSTRAINT title_min_length 
CHECK (LENGTH(title) >= 3);
```

**✅ Validation** :
Testez en essayant d'insérer des données invalides (doit échouer)

---

## **Exercice 2.7 : Transactions SQL** ⭐⭐⭐ AVANCÉ
**Objectif** : Gérer les transactions ACID  
  

**Consigne** :
Scenario : Un utilisateur transfère ses tâches à un autre utilisateur

1. Commencez une transaction
2. Sélectionnez toutes les tâches de l'utilisateur A
3. Transférez-les à l'utilisateur B
4. Si le nombre de tâches transférées = nombre attendu → COMMIT
5. Sinon → ROLLBACK

```sql
BEGIN;

-- Vérification
SELECT COUNT(*) FROM tasks WHERE user_id = 1;

-- Transfert
UPDATE tasks SET user_id = 2 WHERE user_id = 1;

-- Vérification finale
SELECT COUNT(*) FROM tasks WHERE user_id = 2;

-- Si OK
COMMIT;
-- Sinon
ROLLBACK;
```

**✅ Validation** :
- Testez avec un ROLLBACK volontaire
- Vérifiez que les données ne changent pas après ROLLBACK

---

## **Exercice 2.8 : Procédures Stockées** ⭐⭐⭐ AVANCÉ
**Objectif** : Créer une procédure stockée réutilisable  
  

**Consigne** :
Créez une procédure stockée `create_task_with_validation` qui :

1. Vérifie que l'utilisateur existe
2. Vérifie que le titre a au moins 3 caractères
3. Crée la tâche si les validations passent
4. Retourne un message de succès ou d'erreur

```sql
CREATE OR REPLACE FUNCTION create_task_with_validation(
  p_user_id INTEGER,
  p_title VARCHAR,
  p_description TEXT
) RETURNS TEXT AS $$
DECLARE
  v_user_exists BOOLEAN;
BEGIN
  -- Vérification utilisateur
  SELECT EXISTS(SELECT 1 FROM users WHERE id = p_user_id) 
  INTO v_user_exists;
  
  IF NOT v_user_exists THEN
    RETURN 'Error: User does not exist';
  END IF;
  
  -- Vérification titre
  IF LENGTH(p_title) < 3 THEN
    RETURN 'Error: Title must be at least 3 characters';
  END IF;
  
  -- Insertion
  INSERT INTO tasks (user_id, title, description)
  VALUES (p_user_id, p_title, p_description);
  
  RETURN 'Success: Task created';
END;
$$ LANGUAGE plpgsql;
```

**✅ Validation** :
Testez avec différents cas :
- Utilisateur inexistant
- Titre trop court
- Données valides

---

## **Exercice 2.9 : Requêtes d'Agrégation Complexes** ⭐⭐⭐ AVANCÉ
**Objectif** : Maîtriser GROUP BY, HAVING, et fonctions d'agrégation  
  

**Consigne** :
Créez les requêtes suivantes :

1. **Statistiques par utilisateur** :
   ```sql
   user_email | total_tasks | completed_tasks | completion_rate
   ```

2. **Top 5 des utilisateurs les plus productifs** :
   ```sql
   SELECT email, COUNT(*) as task_count
   FROM users u
   JOIN tasks t ON u.id = t.user_id
   GROUP BY u.id, email
   ORDER BY task_count DESC
   LIMIT 5;
   ```

3. **Tâches créées par jour de la semaine** :
   ```sql
   day_of_week | task_count
   ```

4. **Utilisateurs avec plus de 10 tâches et taux de complétion > 50%**

**✅ Validation** :
Chaque requête doit retourner des données agrégées correctes

---

## **Exercice 2.10 : Optimisation et Index** ⭐⭐⭐ AVANCÉ
**Objectif** : Optimiser les performances avec des index  
  

**Consigne** :
1. Créez une table avec 100,000 tâches (script de génération)
2. Exécutez cette requête et notez le temps :
   ```sql
   SELECT * FROM tasks 
   WHERE user_id = 50 AND completed = FALSE;
   ```
3. Utilisez `EXPLAIN ANALYZE` pour voir le plan d'exécution
4. Créez des index appropriés
5. Ré-exécutez et comparez les performances

**Index à créer** :
```sql
CREATE INDEX idx_tasks_user_completed 
ON tasks(user_id, completed);

CREATE INDEX idx_tasks_created_at 
ON tasks(created_at DESC);
```

**✅ Validation** :
- Temps d'exécution réduit d'au moins 50%
- `EXPLAIN ANALYZE` montre l'utilisation des index

**💡 Indice** :
Générez les données avec :
```sql
INSERT INTO tasks (user_id, title, completed)
SELECT 
  (RANDOM() * 100)::INT + 1,
  'Task ' || generate_series,
  RANDOM() > 0.5
FROM generate_series(1, 100000);
```

---

# **CONCEPT 3 : API REST**

## **Exercice 3.1 : Premier Endpoint GET** ⭐ FACILE
**Objectif** : Créer un endpoint API basique  
  

**Consigne** :
Créez un serveur Express avec un endpoint :

```javascript
GET /api/health

Response: 
{
  "status": "OK",
  "timestamp": "2025-12-05T10:30:00Z"
}
```

**✅ Validation** :
```bash
curl http://localhost:5000/api/health
# Doit retourner un JSON avec status et timestamp
```

**Code de départ** :
```javascript
const express = require('express');
const app = express();

// Votre code ici

app.listen(5000, () => {
  console.log('Server running on port 5000');
});
```

---

## **Exercice 3.2 : CRUD Complet - Tasks** ⭐⭐ INTERMÉDIAIRE
**Objectif** : Implémenter les 5 opérations CRUD  
  

**Consigne** :
Implémentez les endpoints suivants (sans base de données, utilisez un tableau en mémoire) :

```javascript
POST   /api/tasks          - Créer une tâche
GET    /api/tasks          - Lister toutes les tâches
GET    /api/tasks/:id      - Récupérer une tâche
PUT    /api/tasks/:id      - Modifier une tâche
DELETE /api/tasks/:id      - Supprimer une tâche
```

**Structure de tâche** :
```javascript
{
  id: 1,
  title: "Apprendre les API REST",
  completed: false,
  createdAt: "2025-12-05T10:00:00Z"
}
```

**✅ Validation** :
Testez avec Postman ou curl :
```bash
# Créer
curl -X POST http://localhost:5000/api/tasks \
  -H "Content-Type: application/json" \
  -d '{"title":"Test task"}'

# Lister
curl http://localhost:5000/api/tasks

# Modifier
curl -X PUT http://localhost:5000/api/tasks/1 \
  -H "Content-Type: application/json" \
  -d '{"completed":true}'
```

---

## **Exercice 3.3 : Validation des Données** ⭐⭐ INTERMÉDIAIRE
**Objectif** : Valider les entrées utilisateur  
  

**Consigne** :
Ajoutez de la validation sur l'endpoint `POST /api/tasks` :

**Règles de validation** :
- `title` : obligatoire, string, 3-100 caractères
- `description` : optionnel, string, max 500 caractères
- `priority` : optionnel, enum ["low", "medium", "high"]
- `dueDate` : optionnel, date valide (format ISO)

**Réponses d'erreur** :
```javascript
// 400 Bad Request
{
  "error": "Validation failed",
  "details": [
    "Title is required",
    "Title must be at least 3 characters",
    "Priority must be one of: low, medium, high"
  ]
}
```

**✅ Validation** :
Testez avec des données invalides :
```bash
# Title trop court
curl -X POST http://localhost:5000/api/tasks \
  -H "Content-Type: application/json" \
  -d '{"title":"ab"}'
# Doit retourner 400 avec message d'erreur
```

**💡 Indice** :
Utilisez une librairie comme `joi` ou `express-validator`

---

## **Exercice 3.4 : Codes de Statut HTTP** ⭐ FACILE
**Objectif** : Utiliser les bons codes HTTP  
  

**Consigne** :
Modifiez vos endpoints pour retourner les bons codes HTTP :

| Situation | Code | Description |
|-----------|------|-------------|
| Création réussie | 201 | Created |
| Récupération réussie | 200 | OK |
| Modification réussie | 200 | OK |
| Suppression réussie | 204 | No Content |
| Ressource non trouvée | 404 | Not Found |
| Données invalides | 400 | Bad Request |
| Erreur serveur | 500 | Internal Server Error |

**✅ Validation** :
```bash
# Créer une tâche
curl -i -X POST http://localhost:5000/api/tasks \
  -H "Content-Type: application/json" \
  -d '{"title":"Test"}'
# HTTP/1.1 201 Created

# Ressource inexistante
curl -i http://localhost:5000/api/tasks/999
# HTTP/1.1 404 Not Found
```

---

## **Exercice 3.5 : Pagination et Filtres** ⭐⭐ INTERMÉDIAIRE
**Objectif** : Implémenter pagination et filtres  
  

**Consigne** :
Améliorez `GET /api/tasks` avec :

**Query parameters** :
- `page` : numéro de page (défaut: 1)
- `limit` : tâches par page (défaut: 10)
- `status` : filter par statut (completed/pending)
- `sortBy` : tri par champ (createdAt/title)
- `order` : ordre (asc/desc)

**Exemple** :
```
GET /api/tasks?page=2&limit=5&status=completed&sortBy=createdAt&order=desc
```

**Réponse** :
```javascript
{
  "data": [ /* 5 tâches */ ],
  "pagination": {
    "page": 2,
    "limit": 5,
    "total": 23,
    "totalPages": 5
  }
}
```

**✅ Validation** :
- Tester sans paramètres (défauts appliqués)
- Tester chaque paramètre individuellement
- Tester combinaisons de paramètres

---

## **Exercice 3.6 : Middleware Personnalisé** ⭐⭐ INTERMÉDIAIRE
**Objectif** : Créer des middlewares réutilisables  
  

**Consigne** :
Créez 3 middlewares :

1. **Logger** : Log toutes les requêtes
   ```
   [2025-12-05 10:30:45] GET /api/tasks - 200
   ```

2. **Request ID** : Ajoute un ID unique à chaque requête
   ```javascript
   req.id = uuid.v4();
   ```

3. **Error Handler** : Gère toutes les erreurs
   ```javascript
   app.use((error, req, res, next) => {
     // Votre code ici
   });
   ```

**✅ Validation** :
- Les logs apparaissent dans la console
- L'ID est présent dans les headers de réponse
- Les erreurs sont gérées proprement

---

## **Exercice 3.7 : Rate Limiting** ⭐⭐⭐ AVANCÉ
**Objectif** : Protéger l'API contre les abus  
  

**Consigne** :
Implémentez un rate limiter :
- **Limite** : 100 requêtes par 15 minutes par IP
- **Réponse** quand limite atteinte :
  ```javascript
  {
    "error": "Too many requests",
    "retryAfter": 600 // secondes
  }
  ```

**Headers de réponse** :
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 42
X-RateLimit-Reset: 1638705000
```

**✅ Validation** :
```bash
# Script de test
for i in {1..101}; do
  curl -i http://localhost:5000/api/tasks
done
# La 101ème requête doit retourner 429
```

**💡 Indice** :
Utilisez `express-rate-limit` ou implémentez avec Redis

---

## **Exercice 3.8 : Versioning d'API** ⭐⭐⭐ AVANCÉ
**Objectif** : Gérer plusieurs versions d'API  
  

**Consigne** :
Créez 2 versions de l'API :

**Version 1** :
```
GET /api/v1/tasks
Response: { tasks: [...] }
```

**Version 2** :
```
GET /api/v2/tasks
Response: { 
  data: [...],
  meta: { version: "2.0" }
}
```

Organisez le code :
```
routes/
├── v1/
│   └── tasks.js
└── v2/
    └── tasks.js
```

**✅ Validation** :
- Les deux versions coexistent
- V1 reste fonctionnelle
- V2 apporte des améliorations

---

## **Exercice 3.9 : Documentation avec Swagger** ⭐⭐⭐ AVANCÉ
**Objectif** : Documenter automatiquement l'API  
  

**Consigne** :
1. Installez `swagger-jsdoc` et `swagger-ui-express`
2. Documentez tous vos endpoints avec JSDoc :

```javascript
/**
 * @swagger
 * /api/tasks:
 *   get:
 *     summary: Récupère toutes les tâches
 *     tags: [Tasks]
 *     parameters:
 *       - in: query
 *         name: status
 *         schema:
 *           type: string
 *           enum: [pending, completed]
 *     responses:
 *       200:
 *         description: Liste des tâches
 *         content:
 *           application/json:
 *             schema:
 *               type: array
 *               items:
 *                 $ref: '#/components/schemas/Task'
 */
router.get('/tasks', getTasksHandler);
```

3. Accédez à la doc sur `/api-docs`

**✅ Validation** :
- Interface Swagger accessible
- Tous les endpoints documentés
- Possibilité de tester depuis l'interface

---

## **Exercice 3.10 : Tests d'Intégration API** ⭐⭐⭐ AVANCÉ
**Objectif** : Tester l'API de bout en bout  
  

**Consigne** :
Écrivez des tests d'intégration avec Jest et Supertest :

```javascript
describe('Tasks API', () => {
  test('POST /api/tasks should create a task', async () => {
    const response = await request(app)
      .post('/api/tasks')
      .send({ title: 'Test task' })
      .expect(201);
    
    expect(response.body).toHaveProperty('id');
    expect(response.body.title).toBe('Test task');
  });
  
  test('GET /api/tasks should return all tasks', async () => {
    await request(app)
      .get('/api/tasks')
      .expect(200)
      .expect('Content-Type', /json/);
  });
  
  // 8 autres tests minimum
});
```

**Tests à couvrir** :
- ✅ Création valide
- ✅ Création avec données invalides
- ✅ Récupération de toutes les tâches
- ✅ Récupération d'une tâche spécifique
- ✅ Tâche non trouvée (404)
- ✅ Modification d'une tâche
- ✅ Suppression d'une tâche
- ✅ Pagination
- ✅ Filtres
- ✅ Rate limiting

**✅ Validation** :
```bash
npm test
# Tous les tests doivent passer
# Couverture > 80%
```

---

# **CONCEPT 4 : AUTHENTIFICATION & SÉCURITÉ**

## **Exercice 4.1 : Hachage de Mot de Passe** ⭐ FACILE
**Objectif** : Hasher les mots de passe avec bcrypt  
  

**Consigne** :
Créez deux fonctions :

```javascript
// Hasher un mot de passe
async function hashPassword(plainPassword) {
  // Votre code ici
}

// Vérifier un mot de passe
async function verifyPassword(plainPassword, hashedPassword) {
  // Votre code ici
}
```

**Tests** :
```javascript
const hashed = await hashPassword('MySecretPass123');
console.log(hashed); // $2b$10$...

const isValid = await verifyPassword('MySecretPass123', hashed);
console.log(isValid); // true

const isInvalid = await verifyPassword('WrongPass', hashed);
console.log(isInvalid); // false
```

**✅ Validation** :
- Le mot de passe n'est jamais stocké en clair
- La vérification fonctionne correctement
- Les hashes sont différents à chaque fois (salt)

---

## **Exercice 4.2 : Endpoint d'Inscription** ⭐⭐ INTERMÉDIAIRE
**Objectif** : Implémenter l'inscription utilisateur  
  

**Consigne** :
Créez `POST /api/auth/register` avec :

**Validation** :
- Email valide et unique
- Mot de passe : min 8 caractères, 1 majuscule, 1 chiffre
- Name : 2-50 caractères

**Réponse succès (201)** :
```javascript
{
  "message": "User registered successfully",
  "user": {
    "id": 1,
    "email": "user@example.com",
    "name": "John Doe"
    // PAS de mot de passe !
  }
}
```

**Réponse erreur (400)** :
```javascript
{
  "error": "Email already exists"
}
```

**✅ Validation** :
```bash
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"SecurePass1","name":"Test"}'
```

---

## **Exercice 4.3 : Génération de JWT** ⭐⭐ INTERMÉDIAIRE
**Objectif** : Créer et signer des tokens JWT  
  

**Consigne** :
Implémentez la génération de JWT :

```javascript
const jwt = require('jsonwebtoken');

function generateToken(userId, email) {
  const payload = {
    userId,
    email,
    iat: Date.now() // issued at
  };
  
  return jwt.sign(
    payload,
    process.env.JWT_SECRET,
    { expiresIn: '24h' }
  );
}
```

Créez aussi `POST /api/auth/login` qui :
1. Vérifie l'email et mot de passe
2. Génère un JWT
3. Retourne le token

**Réponse** :
```javascript
{
  "message": "Login successful",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": 1,
    "email": "user@example.com"
  }
}
```

**✅ Validation** :
- Le token est valide (testez sur jwt.io)
- Le token expire après 24h
- Login avec mauvais credentials retourne 401

---

## **Exercice 4.4 : Middleware d'Authentification** ⭐⭐ INTERMÉDIAIRE
**Objectif** : Protéger les routes avec un middleware  
  

**Consigne** :
Créez un middleware `authMiddleware.js` :

```javascript
function authenticate(req, res, next) {
  // 1. Récupérer le token depuis le header Authorization
  const token = req.headers.authorization?.split(' ')[1];
  
  if (!token) {
    return res.status(401).json({ error: 'No token provided' });
  }
  
  try {
    // 2. Vérifier et décoder le token
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    
    // 3. Ajouter les infos user à la requête
    req.user = decoded;
    
    next();
  } catch (error) {
    return res.status(401).json({ error: 'Invalid token' });
  }
}
```

Protégez les routes tasks :
```javascript
router.get('/tasks', authenticate, getTasksHandler);
router.post('/tasks', authenticate, createTaskHandler);
```

**✅ Validation** :
```bash
# Sans token
curl http://localhost:5000/api/tasks
# 401 Unauthorized

# Avec token
curl http://localhost:5000/api/tasks \
  -H "Authorization: Bearer eyJhbGciOiJI..."
# 200 OK
```

---

## **Exercice 4.5 : Refresh Token** ⭐⭐⭐ AVANCÉ
**Objectif** : Implémenter un système de refresh token  
  

**Consigne** :
Améliorez le système d'auth avec deux tokens :

1. **Access Token** : Courte durée (15 min)
2. **Refresh Token** : Longue durée (7 jours)

**Endpoints** :
```javascript
POST /api/auth/login
Response: {
  accessToken: "...",
  refreshToken: "..."
}

POST /api/auth/refresh
Body: { refreshToken: "..." }
Response: {
  accessToken: "..." // Nouveau token
}
```

**Stockage des refresh tokens** :
```sql
CREATE TABLE refresh_tokens (
  id SERIAL PRIMARY KEY,
  user_id INTEGER REFERENCES users(id),
  token VARCHAR(500) UNIQUE,
  expires_at TIMESTAMP,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**✅ Validation** :
- Access token expire après 15 min
- Refresh token permet d'obtenir un nouveau access token
- Refresh tokens sont révocables (supprimables de la BD)

---

## **Exercice 4.6 : Protection CSRF** ⭐⭐⭐ AVANCÉ
**Objectif** : Protéger contre les attaques CSRF  
  

**Consigne** :
Implémentez la protection CSRF avec `csurf` :

```javascript
const csrf = require('csurf');
const csrfProtection = csrf({ cookie: true });

// Endpoint pour obtenir le token CSRF
app.get('/api/csrf-token', csrfProtection, (req, res) => {
  res.json({ csrfToken: req.csrfToken() });
});

// Protéger les routes POST/PUT/DELETE
app.post('/api/tasks', csrfProtection, createTaskHandler);
```

**Côté client** :
```javascript
// 1. Récupérer le token
const { csrfToken } = await fetch('/api/csrf-token').then(r => r.json());

// 2. L'inclure dans les requêtes
fetch('/api/tasks', {
  method: 'POST',
  headers: {
    'CSRF-Token': csrfToken
  },
  body: JSON.stringify({ title: 'New task' })
});
```

**✅ Validation** :
- Requête sans token CSRF retourne 403
- Requête avec token valide fonctionne

---

## **Exercice 4.7 : Validation et Sanitization** ⭐⭐ INTERMÉDIAIRE
**Objectif** : Protéger contre les injections XSS  
  

**Consigne** :
Nettoyez les entrées utilisateur avec `xss` :

```javascript
const xss = require('xss');

function sanitizeInput(data) {
  if (typeof data === 'string') {
    return xss(data);
  }
  if (typeof data === 'object') {
    const sanitized = {};
    for (let key in data) {
      sanitized[key] = sanitizeInput(data[key]);
    }
    return sanitized;
  }
  return data;
}

// Middleware
app.use((req, res, next) => {
  req.body = sanitizeInput(req.body);
  next();
});
```

**Tests** :
```javascript
// Input dangereux
const malicious = {
  title: '<script>alert("XSS")</script>Hello',
  description: '<img src=x onerror=alert(1)>'
};

// Après sanitization
{
  title: '&lt;script&gt;alert("XSS")&lt;/script&gt;Hello',
  description: '&lt;img src=x onerror=alert(1)&gt;'
}
```

**✅ Validation** :
Les scripts malveillants sont neutralisés

---

## **Exercice 4.8 : Rate Limiting par Utilisateur** ⭐⭐⭐ AVANCÉ
**Objectif** : Limiter les actions par utilisateur  
  

**Consigne** :
Implémentez un rate limiting spécifique :
- Création de tâches : max 20 par heure par utilisateur
- Modification : max 50 par heure
- Suppression : max 10 par heure

```javascript
const rateLimit = require('express-rate-limit');

const createTaskLimiter = rateLimit({
  windowMs: 60 * 60 * 1000, // 1 heure
  max: 20,
  keyGenerator: (req) => req.user.userId, // Par user, pas par IP
  message: 'Too many tasks created, try again later'
});

router.post('/tasks', authenticate, createTaskLimiter, createTaskHandler);
```

**✅ Validation** :
Un utilisateur ne peut pas dépasser les limites

---

## **Exercice 4.9 : Rôles et Permissions (RBAC)** ⭐⭐⭐ AVANCÉ
**Objectif** : Implémenter un système de rôles  
  

**Consigne** :
Créez 3 rôles :
- **user** : CRUD sur ses propres tâches
- **moderator** : Lecture de toutes les tâches
- **admin** : Tout

**Base de données** :
```sql
ALTER TABLE users ADD COLUMN role VARCHAR(20) DEFAULT 'user';
```

**Middleware** :
```javascript
function authorize(...roles) {
  return (req, res, next) => {
    if (!roles.includes(req.user.role)) {
      return res.status(403).json({ 
        error: 'Forbidden: Insufficient permissions' 
      });
    }
    next();
  };
}

// Utilisation
router.get('/admin/users', 
  authenticate, 
  authorize('admin'), 
  getAllUsersHandler
);

router.get('/tasks', 
  authenticate, 
  authorize('user', 'moderator', 'admin'), 
  getTasksHandler
);
```

**✅ Validation** :
- Un user ne peut accéder qu'à ses tâches
- Un admin peut tout faire
- Un moderator peut lire mais pas modifier

---

## **Exercice 4.10 : Audit et Logging de Sécurité** ⭐⭐⭐ AVANCÉ
**Objectif** : Logger les événements de sécurité  
  

**Consigne** :
Créez un système de logging des événements :

**Table audit_logs** :
```sql
CREATE TABLE audit_logs (
  id SERIAL PRIMARY KEY,
  user_id INTEGER,
  action VARCHAR(50),
  resource VARCHAR(50),
  ip_address VARCHAR(45),
  user_agent TEXT,
  status VARCHAR(20),
  details JSONB,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Événements à logger** :
- Login réussi/échoué
- Inscription
- Création/Modification/Suppression de ressources
- Tentatives d'accès non autorisé

**Middleware** :
```javascript
function auditLog(action, resource) {
  return async (req, res, next) => {
    const originalSend = res.send;
    
    res.send = function(data) {
      // Logger après la réponse
      logAuditEvent({
        userId: req.user?.userId,
        action,
        resource,
        ip: req.ip,
        userAgent: req.get('User-Agent'),
        status: res.statusCode < 400 ? 'success' : 'failure'
      });
      
      originalSend.call(this, data);
    };
    
    next();
  };
}

// Utilisation
router.post('/tasks', 
  authenticate, 
  auditLog('CREATE', 'task'), 
  createTaskHandler
);
```

**✅ Validation** :
- Tous les événements sont loggés
- Dashboard admin pour voir les logs
- Filtrage par utilisateur, action, date

---

# **CONCEPT 5 : FRONTEND REACT**

## **Exercice 5.1 : Composant de Base** ⭐ FACILE
**Objectif** : Créer un composant React simple  
 

**Consigne** :
Créez un composant `TaskItem` :

```jsx
function TaskItem({ task, onToggle, onDelete }) {
  return (
    <div className="task-item">
      <input 
        type="checkbox" 
        checked={task.completed}
        onChange={() => onToggle(task.id)}
      />
      <span>{task.title}</span>
      <button onClick={() => onDelete(task.id)}>Delete</button>
    </div>
  );
}
```

**✅ Validation** :
- Le composant s'affiche correctement
- Les callbacks fonctionnent
- Props sont typés (PropTypes ou TypeScript)

---

## **Exercice 5.2 : Gestion d'État avec useState** ⭐ FACILE
**Objectif** : Gérer l'état local d'un composant  
  

**Consigne** :
Créez un formulaire de création de tâche :

```jsx
function TaskForm({ onSubmit }) {
  const [title, setTitle] = useState('');
  const [description, setDescription] = useState('');
  
  const handleSubmit = (e) => {
    e.preventDefault();
    if (title.trim()) {
      onSubmit({ title, description });
      setTitle('');
      setDescription('');
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      {/* Champs du formulaire */}
    </form>
  );
}
```

**✅ Validation** :
- Le formulaire se réinitialise après soumission
- Validation côté client (titre obligatoire)
- Feedback visuel sur les erreurs

---

## **Exercice 5.3 : Appels API avec useEffect** ⭐⭐ INTERMÉDIAIRE
**Objectif** : Charger des données depuis l'API  
  

**Consigne** :
Créez un composant `TaskList` qui charge les tâches :

```jsx
function TaskList() {
  const [tasks, setTasks] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    async function fetchTasks() {
      try {
        setLoading(true);
        const response = await fetch('/api/tasks');
        const data = await response.json();
        setTasks(data);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    }
    
    fetchTasks();
  }, []);
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  
  return (
    <div>
      {tasks.map(task => (
        <TaskItem key={task.id} task={task} />
      ))}
    </div>
  );
}
```

**✅ Validation** :
- Les tâches s'affichent après chargement
- Loading spinner pendant le chargement
- Message d'erreur si échec

---

## **Exercice 5.4 : Context API pour l'Authentification** ⭐⭐ INTERMÉDIAIRE
**Objectif** : Gérer l'état global avec Context  
  

**Consigne** :
Créez un `AuthContext` :

```jsx
const AuthContext = createContext();

export function AuthProvider({ children }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    // Vérifier si l'utilisateur est connecté
    const token = localStorage.getItem('token');
    if (token) {
      // Valider le token et charger l'utilisateur
      loadUser(token);
    } else {
      setLoading(false);
    }
  }, []);
  
  const login = async (email, password) => {
    const response = await fetch('/api/auth/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email, password })
    });
    
    const data = await response.json();
    localStorage.setItem('token', data.token);
    setUser(data.user);
  };
  
  const logout = () => {
    localStorage.removeItem('token');
    setUser(null);
  };
  
  return (
    <AuthContext.Provider value={{ user, login, logout, loading }}>
      {children}
    </AuthContext.Provider>
  );
}

export function useAuth() {
  return useContext(AuthContext);
}
```

**✅ Validation** :
- Login/logout fonctionnent
- L'état persiste après rafraîchissement
- Routes protégées redirigent si non connecté

---

## **Exercice 5.5 : Formulaire avec Validation** ⭐⭐ INTERMÉDIAIRE
**Objectif** : Valider un formulaire côté client  
  

**Consigne** :
Créez un formulaire d'inscription avec validation :

**Règles** :
- Email : format valide
- Mot de passe : min 8 caractères, 1 majuscule, 1 chiffre
- Confirmation mot de passe : identique
- Nom : 2-50 caractères

**Affichage** :
- Messages d'erreur sous chaque champ
- Bouton désactivé si formulaire invalide
- Indicateur de force du mot de passe

```jsx
function RegisterForm() {
  const [formData, setFormData] = useState({
    email: '',
    password: '',
    confirmPassword: '',
    name: ''
  });
  
  const [errors, setErrors] = useState({});
  
  const validate = () => {
    const newErrors = {};
    
    // Email
    if (!/\S+@\S+\.\S+/.test(formData.email)) {
      newErrors.email = 'Email invalide';
    }
    
    // Password
    if (formData.password.length < 8) {
      newErrors.password = 'Minimum 8 caractères';
    }
    
    // ... autres validations
    
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };
  
  // ...
}
```

**✅ Validation** :
- Validation en temps réel (ou onBlur)
- Messages d'erreur clairs
- UX intuitive

---

## **Exercice 5.6 : Custom Hook useApi** ⭐⭐⭐ AVANCÉ
**Objectif** : Créer un hook réutilisable pour les appels API  
  

**Consigne** :
Créez un hook `useApi` :

```jsx
function useApi(url, options = {}) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  
  const execute = useCallback(async (overrideOptions = {}) => {
    try {
      setLoading(true);
      setError(null);
      
      const token = localStorage.getItem('token');
      const response = await fetch(url, {
        ...options,
        ...overrideOptions,
        headers: {
          'Content-Type': 'application/json',
          'Authorization': token ? `Bearer ${token}` : '',
          ...options.headers,
          ...overrideOptions.headers
        }
      });
      
      if (!response.ok) {
        throw new Error(`HTTP ${response.status}`);
      }
      
      const result = await response.json();
      setData(result);
      return result;
    } catch (err) {
      setError(err.message);
      throw err;
    } finally {
      setLoading(false);
    }
  }, [url, options]);
  
  return { data, loading, error, execute };
}

// Utilisation
function TaskList() {
  const { data: tasks, loading, error, execute } = useApi('/api/tasks');
  
  useEffect(() => {
    execute();
  }, [execute]);
  
  const deleteTask = async (id) => {
    await execute({ method: 'DELETE', url: `/api/tasks/${id}` });
    // Recharger la liste
    execute();
  };
  
  // ...
}
```

**✅ Validation** :
- Hook réutilisable pour tous les appels API
- Gestion automatique du loading/error
- Support des différentes méthodes HTTP

---

## **Exercice 5.7 : Optimisation avec React.memo** ⭐⭐⭐ AVANCÉ
**Objectif** : Optimiser les performances  
  

**Consigne** :
Optimisez le rendu de la liste de tâches :

```jsx
const TaskItem = React.memo(({ task, onToggle, onDelete }) => {
  console.log(`Rendering task ${task.id}`);
  
  return (
    <div className="task-item">
      <input 
        type="checkbox" 
        checked={task.completed}
        onChange={() => onToggle(task.id)}
      />
      <span>{task.title}</span>
      <button onClick={() => onDelete(task.id)}>Delete</button>
    </div>
  );
}, (prevProps, nextProps) => {
  // Custom comparison
  return prevProps.task.id === nextProps.task.id &&
         prevProps.task.completed === nextProps.task.completed &&
         prevProps.task.title === nextProps.task.title;
});

function TaskList() {
  const [tasks, setTasks] = useState([]);
  
  // Mémoiser les callbacks
  const handleToggle = useCallback((id) => {
    setTasks(prev => prev.map(task =>
      task.id === id ? { ...task, completed: !task.completed } : task
    ));
  }, []);
  
  const handleDelete = useCallback((id) => {
    setTasks(prev => prev.filter(task => task.id !== id));
  }, []);
  
  return (
    <div>
      {tasks.map(task => (
        <TaskItem 
          key={task.id} 
          task={task}
          onToggle={handleToggle}
          onDelete={handleDelete}
        />
      ))}
    </div>
  );
}
```

**✅ Validation** :
- Seuls les composants modifiés re-rendent
- Vérifiez avec React DevTools Profiler
- Performance améliorée sur grandes listes

---

## **Exercice 5.8 : Routing avec React Router** ⭐⭐ INTERMÉDIAIRE
**Objectif** : Gérer la navigation  
  

**Consigne** :
Créez les routes suivantes :

```jsx
import { BrowserRouter, Routes, Route, Navigate } from 'react-router-dom';

function App() {
  return (
    <AuthProvider>
      <BrowserRouter>
        <Routes>
          <Route path="/login" element={<LoginPage />} />
          <Route path="/register" element={<RegisterPage />} />
          
          <Route element={<ProtectedRoute />}>
            <Route path="/dashboard" element={<DashboardPage />} />
            <Route path="/tasks" element={<TasksPage />} />
            <Route path="/tasks/:id" element={<TaskDetailPage />} />
            <Route path="/profile" element={<ProfilePage />} />
          </Route>
          
          <Route path="/" element={<Navigate to="/dashboard" />} />
          <Route path="*" element={<NotFoundPage />} />
        </Routes>
      </BrowserRouter>
    </AuthProvider>
  );
}

function ProtectedRoute() {
  const { user, loading } = useAuth();
  const location = useLocation();
  
  if (loading) return <div>Loading...</div>;
  
  return user ? (
    <Outlet />
  ) : (
    <Navigate to="/login" state={{ from: location }} replace />
  );
}
```

**✅ Validation** :
- Navigation fonctionnelle
- Redirections automatiques si non connecté
- Retour à la page d'origine après login
- Page 404 pour routes inexistantes

---

## **Exercice 5.9 : Tests avec React Testing Library** ⭐⭐⭐ AVANCÉ
**Objectif** : Tester les composants React  
  

**Consigne** :
Écrivez des tests pour `TaskForm` :

```jsx
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import TaskForm from './TaskForm';

describe('TaskForm', () => {
  test('renders form elements', () => {
    render(<TaskForm onSubmit={jest.fn()} />);
    
    expect(screen.getByLabelText(/title/i)).toBeInTheDocument();
    expect(screen.getByLabelText(/description/i)).toBeInTheDocument();
    expect(screen.getByRole('button', { name: /create/i })).toBeInTheDocument();
  });
  
  test('calls onSubmit with form data', async () => {
    const mockSubmit = jest.fn();
    render(<TaskForm onSubmit={mockSubmit} />);
    
    await userEvent.type(screen.getByLabelText(/title/i), 'New Task');
    await userEvent.type(screen.getByLabelText(/description/i), 'Description');
    
    fireEvent.click(screen.getByRole('button', { name: /create/i }));
    
    await waitFor(() => {
      expect(mockSubmit).toHaveBeenCalledWith({
        title: 'New Task',
        description: 'Description'
      });
    });
  });
  
  test('shows validation error for empty title', async () => {
    render(<TaskForm onSubmit={jest.fn()} />);
    
    fireEvent.click(screen.getByRole('button', { name: /create/i }));
    
    expect(await screen.findByText(/title is required/i)).toBeInTheDocument();
  });
  
  // 5 autres tests minimum
});
```

**Tests à couvrir** :
- ✅ Rendu des éléments
- ✅ Soumission avec données valides
- ✅ Validation d'erreurs
- ✅ Réinitialisation après soumission
- ✅ Désactivation du bouton si invalide
- ✅ Compteur de caractères
- ✅ Gestion des erreurs API
- ✅ Accessibilité (labels, ARIA)

**✅ Validation** :
```bash
npm test -- --coverage
# Couverture > 80%
```

---

## **Exercice 5.10 : Application Complète avec State Management** ⭐⭐⭐ AVANCÉ
**Objectif** : Intégrer tous les concepts  
  

**Consigne** :
Créez l'application TaskFlow complète avec :

**Features** :
1. **Authentification**
   - Login/Register
   - Persistance de session
   - Logout

2. **Dashboard**
   - Statistiques (total, terminées, en cours)
   - Graphique de progression

3. **Liste de tâches**
   - Affichage paginé
   - Filtres (statut, priorité)
   - Recherche en temps réel
   - Tri (date, titre, priorité)

4. **CRUD Tâches**
   - Création avec modal
   - Modification inline
   - Suppression avec confirmation
   - Toggle completed

5. **UX/UI**
   - Design responsive (mobile-first)
   - Loading skeletons
   - Toasts de notification
   - Animations de transition

**Architecture** :
```
src/
├── components/
│   ├── Auth/
│   ├── Tasks/
│   ├── Layout/
│   └── Common/
├── contexts/
│   └── AuthContext.jsx
├── hooks/
│   ├── useApi.js
│   ├── useAuth.js
│   └── useTasks.js
├── pages/
│   ├── Login.jsx
│   ├── Dashboard.jsx
│   └── Tasks.jsx
├── services/
│   └── api.js
├── utils/
│   └── helpers.js
└── App.jsx
```

**✅ Validation** :
- Toutes les fonctionnalités fonctionnent
- Tests E2E avec Cypress (5 scénarios)
- Application déployée
- Score Lighthouse > 90

---

# **CONCEPT 6 : TESTS**

## **Exercice 6.1 : Premier Test Unitaire** ⭐ FACILE
**Objectif** : Écrire un test Jest basique  
 

**Consigne** :
Testez une fonction utilitaire :

```javascript
// utils/validators.js
export function isValidEmail(email) {
  const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return regex.test(email);
}

// utils/validators.test.js
import { isValidEmail } from './validators';

describe('isValidEmail', () => {
  test('returns true for valid email', () => {
    expect(isValidEmail('test@example.com')).toBe(true);
  });
  
  test('returns false for invalid email', () => {
    expect(isValidEmail('invalid-email')).toBe(false);
    expect(isValidEmail('test@')).toBe(false);
    expect(isValidEmail('@example.com')).toBe(false);
  });
  
  test('returns false for empty string', () => {
    expect(isValidEmail('')).toBe(false);
  });
});
```

**✅ Validation** :
```bash
npm test validators.test.js
# Tous les tests passent
```

---

## **Exercice 6.2 : Tests avec Mocks** ⭐⭐ INTERMÉDIAIRE
**Objectif** : Utiliser jest.fn() et mocks  
  

**Consigne** :
Testez un service qui fait des appels API :

```javascript
// services/taskService.js
export async function getTasks() {
  const response = await fetch('/api/tasks');
  return response.json();
}

// services/taskService.test.js
import { getTasks } from './taskService';

global.fetch = jest.fn();

describe('getTasks', () => {
  beforeEach(() => {
    fetch.mockClear();
  });
  
  test('fetches tasks successfully', async () => {
    const mockTasks = [
      { id: 1, title: 'Task 1' },
      { id: 2, title: 'Task 2' }
    ];
    
    fetch.mockResolvedValueOnce({
      json: async () => mockTasks
    });
    
    const tasks = await getTasks();
    
    expect(fetch).toHaveBeenCalledWith('/api/tasks');
    expect(tasks).toEqual(mockTasks);
  });
  
  test('handles fetch error', async () => {
    fetch.mockRejectedValueOnce(new Error('Network error'));
    
    await expect(getTasks()).rejects.toThrow('Network error');
  });
});
```

**✅ Validation** :
- Les mocks sont utilisés correctement
- Les appels API ne sont pas réels
- Tous les cas (succès/erreur) sont testés

---

## **Exercice 6.3 : Tests d'Intégration Backend** ⭐⭐ INTERMÉDIAIRE
**Objectif** : Tester l'API de bout en bout  
  

**Consigne** :
Testez les endpoints avec Supertest :

```javascript
import request from 'supertest';
import app from '../app';

describe('Tasks API', () => {
  let authToken;
  let taskId;
  
  beforeAll(async () => {
    // Créer un utilisateur de test et obtenir un token
    const response = await request(app)
      .post('/api/auth/register')
      .send({
        email: 'test@example.com',
        password: 'TestPass123',
        name: 'Test User'
      });
    
    authToken = response.body.token;
  });
  
  test('POST /api/tasks creates a task', async () => {
    const response = await request(app)
      .post('/api/tasks')
      .set('Authorization', `Bearer ${authToken}`)
      .send({
        title: 'Test Task',
        description: 'Test Description'
      })
      .expect(201);
    
    expect(response.body).toHaveProperty('id');
    expect(response.body.title).toBe('Test Task');
    
    taskId = response.body.id;
  });
  
  test('GET /api/tasks returns tasks', async () => {
    const response = await request(app)
      .get('/api/tasks')
      .set('Authorization', `Bearer ${authToken}`)
      .expect(200);
    
    expect(Array.isArray(response.body)).toBe(true);
    expect(response.body.length).toBeGreaterThan(0);
  });
  
  // 8 autres tests minimum
});
```

**✅ Validation** :
- Tous les endpoints sont testés
- Tests isolés (chaque test nettoie après lui)
- Couverture > 80%

---

## **Exercice 6.4 : Test-Driven Development (TDD)** ⭐⭐⭐ AVANCÉ
**Objectif** : Développer en TDD (test first)  
  

**Consigne** :
Implémentez une fonction de statistiques EN COMMENÇANT PAR LES TESTS :

**1. Écrire les tests d'abord** :
```javascript
// utils/stats.test.js
import { calculateTaskStats } from './stats';

describe('calculateTaskStats', () => {
  test('calculates stats for empty array', () => {
    const stats = calculateTaskStats([]);
    
    expect(stats).toEqual({
      total: 0,
      completed: 0,
      pending: 0,
      completionRate: 0
    });
  });
  
  test('calculates stats correctly', () => {
    const tasks = [
      { id: 1, completed: true },
      { id: 2, completed: false },
      { id: 3, completed: true },
      { id: 4, completed: true }
    ];
    
    const stats = calculateTaskStats(tasks);
    
    expect(stats).toEqual({
      total: 4,
      completed: 3,
      pending: 1,
      completionRate: 75
    });
  });
  
  // 5 autres tests
});
```

**2. Les tests échouent (RED)** ✗

**3. Implémenter le code minimum** :
```javascript
// utils/stats.js
export function calculateTaskStats(tasks) {
  if (tasks.length === 0) {
    return { total: 0, completed: 0, pending: 0, completionRate: 0 };
  }
  
  const completed = tasks.filter(t => t.completed).length;
  const pending = tasks.length - completed;
  const completionRate = Math.round((completed / tasks.length) * 100);
  
  return {
    total: tasks.length,
    completed,
    pending,
    completionRate
  };
}
```

**4. Les tests passent (GREEN)** ✓

**5. Refactorer si nécessaire (REFACTOR)**

**✅ Validation** :
- Vous avez suivi le cycle RED-GREEN-REFACTOR
- Tous les tests passent
- Code propre et optimisé

---

## **Exercice 6.5 : Tests E2E avec Cypress** ⭐⭐⭐ AVANCÉ
**Objectif** : Tester des scénarios utilisateur complets  
  

**Consigne** :
Écrivez 5 tests E2E avec Cypress :

```javascript
// cypress/e2e/tasks.cy.js
describe('Task Management', () => {
  beforeEach(() => {
    // Réinitialiser la BD
    cy.task('db:seed');
    
    // Se connecter
    cy.visit('/login');
    cy.get('[data-testid="email"]').type('test@example.com');
    cy.get('[data-testid="password"]').type('TestPass123');
    cy.get('[data-testid="login-button"]').click();
    
    cy.url().should('include', '/dashboard');
  });
  
  it('creates a new task', () => {
    cy.get('[data-testid="create-task-button"]').click();
    
    cy.get('[data-testid="task-title"]').type('New Task');
    cy.get('[data-testid="task-description"]').type('Description');
    cy.get('[data-testid="submit-task"]').click();
    
    cy.contains('New Task').should('be.visible');
    cy.contains('Task created successfully').should('be.visible');
  });
  
  it('completes a task', () => {
    cy.get('[data-testid="task-checkbox"]').first().click();
    
    cy.get('[data-testid="task-item"]').first()
      .should('have.class', 'completed');
  });
  
  it('filters tasks by status', () => {
    cy.get('[data-testid="filter-completed"]').click();
    
    cy.get('[data-testid="task-item"]').each($task => {
      expect($task).to.have.class('completed');
    });
  });
  
  it('searches for tasks', () => {
    cy.get('[data-testid="search-input"]').type('important');
    
    cy.get('[data-testid="task-item"]').each($task => {
      expect($task.text().toLowerCase()).to.include('important');
    });
  });
  
  it('deletes a task with confirmation', () => {
    const taskTitle = 'Task to delete';
    
    cy.contains(taskTitle).parent()
      .find('[data-testid="delete-button"]').click();
    
    cy.get('[data-testid="confirm-delete"]').click();
    
    cy.contains(taskTitle).should('not.exist');
  });
});
```

**✅ Validation** :
- Tests passent en mode headless
- Vidéos des tests disponibles
- Screenshots des échecs

---

## **Exercice 6.6 : Code Coverage** ⭐⭐ INTERMÉDIAIRE
**Objectif** : Atteindre 80% de couverture  
  

**Consigne** :
1. Exécutez les tests avec couverture :
```bash
npm test -- --coverage
```

2. Identifiez les zones non couvertes

3. Ajoutez des tests pour atteindre 80% sur :
   - Statements
   - Branches
   - Functions
   - Lines

4. Générez le rapport HTML :
```bash
npm test -- --coverage --coverageReporters=html
```

**✅ Validation** :
```
----------------------|---------|----------|---------|---------|
File                  | % Stmts | % Branch | % Funcs | % Lines |
----------------------|---------|----------|---------|---------|
All files             |   85.23 |    82.41 |   88.76 |   85.12 |
 controllers          |   90.15 |    85.32 |   92.45 |   90.23 |
  taskController.js   |   92.34 |    88.12 |   95.23 |   92.11 |
 services             |   88.45 |    82.15 |   87.34 |   88.23 |
  taskService.js      |   89.23 |    83.45 |   88.67 |   89.12 |
----------------------|---------|----------|---------|---------|
```

---

## **Exercice 6.7 : Tests de Performance** ⭐⭐⭐ AVANCÉ
**Objectif** : Tester les performances de l'API  
  

**Consigne** :
Utilisez `artillery` ou `k6` pour tester la charge :

```yaml
# artillery-config.yml
config:
  target: 'http://localhost:5000'
  phases:
    - duration: 60
      arrivalRate: 10
      name: "Warm up"
    - duration: 120
      arrivalRate: 50
      name: "Sustained load"
    - duration: 60
      arrivalRate: 100
      name: "Peak load"

scenarios:
  - name: "Get tasks"
    flow:
      - get:
          url: "/api/tasks"
          headers:
            Authorization: "Bearer {{ token }}"
      - think: 2
```

**Métriques à mesurer** :
- Temps de réponse moyen
- P95, P99 (95e et 99e percentiles)
- Taux d'erreur
- Requêtes par seconde

**Critères de succès** :
- P95 < 200ms
- P99 < 500ms
- Taux d'erreur < 1%
- Supporte 100 req/s

**✅ Validation** :
- Rapport de performance généré
- Tous les critères sont atteints
- Optimisations documentées si nécessaire

---

## **Exercice 6.8 : Tests de Sécurité** ⭐⭐⭐ AVANCÉ
**Objectif** : Tester la sécurité de l'application  
  

**Consigne** :
Créez des tests de sécurité :

```javascript
describe('Security Tests', () => {
  describe('SQL Injection', () => {
    test('prevents SQL injection in search', async () => {
      const malicious = "'; DROP TABLE tasks; --";
      
      const response = await request(app)
        .get(`/api/tasks/search?q=${encodeURIComponent(malicious)}`)
        .set('Authorization', `Bearer ${token}`)
        .expect(200);
      
      // L'application ne doit pas crasher
      expect(response.body).toBeDefined();
      
      // La table doit toujours exister
      const tasks = await request(app)
        .get('/api/tasks')
        .set('Authorization', `Bearer ${token}`);
      
      expect(tasks.status).toBe(200);
    });
  });
  
  describe('XSS Protection', () => {
    test('sanitizes XSS in task title', async () => {
      const xss = '<script>alert("XSS")</script>';
      
      const response = await request(app)
        .post('/api/tasks')
        .set('Authorization', `Bearer ${token}`)
        .send({ title: xss })
        .expect(201);
      
      expect(response.body.title).not.toContain('<script>');
    });
  });
  
  describe('Authentication', () => {
    test('requires authentication', async () => {
      await request(app)
        .get('/api/tasks')
        .expect(401);
    });
    
    test('rejects invalid token', async () => {
      await request(app)
        .get('/api/tasks')
        .set('Authorization', 'Bearer invalid_token')
        .expect(401);
    });
  });
  
  describe('Authorization', () => {
    test('prevents access to other users tasks', async () => {
      // Utilisateur 1 crée une tâche
      const task = await createTask(user1Token);
      
      // Utilisateur 2 tente d'y accéder
      await request(app)
        .get(`/api/tasks/${task.id}`)
        .set('Authorization', `Bearer ${user2Token}`)
        .expect(403);
    });
  });
  
  // 10 autres tests de sécurité
});
```

**✅ Validation** :
- Toutes les vulnérabilités OWASP Top 10 sont testées
- L'application est sécurisée

---

## **Exercice 6.9 : Tests de Régression** ⭐⭐ INTERMÉDIAIRE
**Objectif** : Détecter les régressions  
  

**Consigne** :
1. Créez une suite de tests de régression complète
2. Exécutez avant chaque déploiement
3. Bloquez le déploiement si échec

```javascript
// tests/regression.test.js
describe('Regression Tests', () => {
  describe('Critical Paths', () => {
    test('User can complete full workflow', async () => {
      // 1. Register
      const user = await register();
      
      // 2. Login
      const token = await login(user);
      
      // 3. Create task
      const task = await createTask(token);
      
      // 4. Update task
      await updateTask(token, task.id);
      
      // 5. Complete task
      await completeTask(token, task.id);
      
      // 6. Verify stats
      const stats = await getStats(token);
      expect(stats.completed).toBeGreaterThan(0);
    });
    
    // 10 autres parcours critiques
  });
  
  describe('Known Bugs - Fixed', () => {
    test('Bug #42: Task duplication on double click', async () => {
      // Reproduire le bug
      // Vérifier qu'il est corrigé
    });
    
    // Tests pour tous les bugs résolus
  });
});
```

**✅ Validation** :
- Suite de tests exhaustive
- Documentation des bugs corrigés
- Intégré dans le CI/CD

---

## **Exercice 6.10 : Test Automation Framework** ⭐⭐⭐ AVANCÉ
**Objectif** : Créer un framework de tests complet  
  

**Consigne** :
Créez un framework de tests réutilisable :

**Structure** :
```
tests/
├── unit/
│   ├── controllers/
│   ├── services/
│   └── utils/
├── integration/
│   └── api/
├── e2e/
│   └── scenarios/
├── performance/
├── security/
├── fixtures/
│   ├── users.js
│   └── tasks.js
├── helpers/
│   ├── setup.js
│   ├── teardown.js
│   └── factories.js
└── config/
    ├── jest.config.js
    └── cypress.config.js
```

**Helpers réutilisables** :
```javascript
// helpers/factories.js
export class TaskFactory {
  static create(overrides = {}) {
    return {
      title: faker.lorem.words(3),
      description: faker.lorem.paragraph(),
      completed: false,
      priority: 'medium',
      ...overrides
    };
  }
  
  static createMany(count, overrides = {}) {
    return Array.from({ length: count }, () => this.create(overrides));
  }
}

// helpers/setup.js
export async function setupTestEnvironment() {
  await connectToTestDB();
  await clearDatabase();
  await seedTestData();
}
```

**Scripts npm** :
```json
{
  "scripts": {
    "test": "jest",
    "test:unit": "jest --testPathPattern=unit",
    "test:integration": "jest --testPathPattern=integration",
    "test:e2e": "cypress run",
    "test:all": "npm run test:unit && npm run test:integration && npm run test:e2e",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage"
  }
}
```

**✅ Validation** :
- Framework complet et documenté
- Tous les types de tests inclus
- Facile à étendre et maintenir
- Documentation pour les nouveaux développeurs

---
