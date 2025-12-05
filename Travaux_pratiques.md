# Exercices - Conception et Développement d'Applications Sécurisées

## 📋 Table des Matières
1. [Authentification et Gestion des Sessions](#1-authentification-et-gestion-des-sessions)
2. [Validation et Sanitisation des Entrées](#2-validation-et-sanitisation-des-entrées)
3. [Injections SQL et NoSQL](#3-injections-sql-et-nosql)
4. [XSS (Cross-Site Scripting)](#4-xss-cross-site-scripting)
5. [CSRF (Cross-Site Request Forgery)](#5-csrf-cross-site-request-forgery)
6. [Cryptographie et Hachage](#6-cryptographie-et-hachage)
7. [Gestion des Permissions et Autorisations](#7-gestion-des-permissions-et-autorisations)
8. [Sécurité des APIs REST](#8-sécurité-des-apis-rest)
9. [HTTPS et Certificats SSL/TLS](#9-https-et-certificats-ssltls)
10. [Logging et Monitoring de Sécurité](#10-logging-et-monitoring-de-sécurité)

---

## 1. Authentification et Gestion des Sessions

### 🟢 Niveau Facile

**Exercice 1.1 : Premier Login**
```
Créez un système de login basique qui :
- Accepte un username et un password
- Compare avec des credentials hardcodés
- Retourne "Connexion réussie" ou "Échec"

Contraintes :
- 1 seul utilisateur (admin/password123)
- Pas de base de données
```

**Exercice 1.2 : Session Simple**
```
Ajoutez la gestion de session :
- Générez un token aléatoire à la connexion
- Stockez-le dans un dictionnaire {token: username}
- Créez une fonction is_logged_in(token)

Test : Vérifiez qu'un utilisateur avec token valide peut accéder
```

**Exercice 1.3 : Timeout de Session**
```
Implémentez l'expiration de session :
- Stockez timestamp de création avec chaque token
- Session expire après 5 minutes
- Fonction cleanup_expired_sessions()

Défi : Testez avec plusieurs utilisateurs simultanés
```

### 🟡 Niveau Intermédiaire

**Exercice 1.4 : Multi-utilisateurs avec Hachage**
```
Système complet avec :
- Base de données SQLite (users table)
- Mots de passe hachés avec bcrypt
- Fonction register(username, password)
- Fonction login(username, password)

Bonus : Empêchez les doublons de username
```

**Exercice 1.5 : Refresh Token**
```
Implémentez le double token :
- Access Token (15 min de validité)
- Refresh Token (7 jours de validité)
- Endpoint /refresh pour renouveler l'access token
- Stockage sécurisé des refresh tokens

Pattern : Access token en mémoire, refresh en DB
```

**Exercice 1.6 : Limitation des Tentatives**
```
Protégez contre brute force :
- Comptez les tentatives échouées par IP/username
- Bloquez après 5 échecs pendant 15 minutes
- Utilisez Redis ou un dictionnaire en mémoire
- Endpoint /unlock pour admin

Challenge : Testez avec 100 tentatives automatisées
```

### 🔴 Niveau Avancé

**Exercice 1.7 : JWT Complet**
```
Implémentez JWT (JSON Web Tokens) :
- Créez access et refresh tokens JWT
- Incluez claims : user_id, roles, exp
- Signez avec RS256 (paires de clés)
- Validez signature et expiration
- Endpoint /me pour info utilisateur

Architecture : Suivez RFC 7519
```

**Exercice 1.8 : OAuth 2.0 Flow**
```
Simulez le flow Authorization Code :
1. Client demande authorization
2. User consent screen
3. Authorization code généré
4. Échange code contre tokens
5. Utilisation access token

Implémentez : /authorize, /token, /userinfo
```

**Exercice 1.9 : MFA (Multi-Factor Authentication)**
```
Ajoutez l'authentification à deux facteurs :
- Générez secret TOTP (Time-based OTP)
- Affichez QR code pour Google Authenticator
- Validez code à 6 chiffres
- Codes de backup (10 codes one-time)
- Table mfa_secrets en DB

Librairie : pyotp ou speakeasy
```

**Exercice 1.10 : Session Hijacking Prevention**
```
Protections avancées :
- Bind session à User-Agent et IP
- Rotation de session ID après login
- Secure et HttpOnly flags sur cookies
- SameSite=Strict
- Détection de sessions parallèles suspectes

Test : Tentez de voler une session via XSS
```

---

## 2. Validation et Sanitisation des Entrées

### 🟢 Niveau Facile

**Exercice 2.1 : Validation Email**
```
Fonction validate_email(email) :
- Vérifie présence de @ et .
- Minimum 3 caractères avant @
- Retourne True/False

Tests :
✓ "user@example.com"
✗ "invalid.email"
✗ "@example.com"
```

**Exercice 2.2 : Nettoyage HTML**
```
Fonction strip_html(text) :
- Supprime toutes les balises HTML
- Garde uniquement le texte
- Utilise regex ou bibliothèque

Exemple : "<p>Hello</p>" → "Hello"
```

**Exercice 2.3 : Validation Numérique**
```
Fonction validate_age(input) :
- Accepte uniquement des nombres
- Range : 0-150
- Rejette valeurs négatives et décimales

Contrainte : Gérez les inputs string
```

### 🟡 Niveau Intermédiaire

**Exercice 2.4 : Schema Validation JSON**
```
Validez un objet User avec jsonschema :
{
  "username": string (3-20 chars, alphanumeric),
  "email": string (format email),
  "age": integer (18-100),
  "role": enum ["user", "admin"]
}

Retournez messages d'erreur détaillés
```

**Exercice 2.5 : Upload Fichier Sécurisé**
```
Fonction validate_upload(file) :
- Extensions autorisées : jpg, png, pdf
- Taille max : 5MB
- Vérifiez magic bytes (vraie extension)
- Renommez avec UUID
- Stockez hors webroot

Testez : Upload d'un fichier .php renommé en .jpg
```

**Exercice 2.6 : Sanitisation SQL**
```
Fonction safe_query(table, conditions) :
- Utilisez parameterized queries
- Whitelist des noms de tables
- Échappez les identifiers
- Empêchez UNION, DROP, etc.

Exemple : safe_query("users", {"id": user_input})
```

**Exercice 2.7 : Path Traversal Prevention**
```
Fonction secure_path(user_path, base_dir) :
- Résolvez chemins absolus
- Vérifiez que path reste dans base_dir
- Bloquez .., symblinks malveillants
- Normalisez le chemin

Test : Bloquez "../../../../etc/passwd"
```

### 🔴 Niveau Avancé

**Exercice 2.8 : Content Security Policy**
```
Implémentez CSP headers :
- Définissez politique stricte
- script-src : self + nonce
- img-src : self + https://cdn.example.com
- Bloquez inline scripts
- Rapport des violations à /csp-report

Testez avec OWASP ZAP
```

**Exercice 2.9 : Input Validation Framework**
```
Créez un framework de validation :
- Decorateurs @validate(schema)
- Validators chainables (required, min, max, regex)
- Messages d'erreur i18n
- Validation async (check email exists)

Exemple :
@validate({
  "email": [required(), email(), unique("users")]
})
def register(data):
  ...
```

**Exercice 2.10 : Fuzzing Automatique**
```
Testez vos validations avec fuzzing :
- Générez 1000 inputs aléatoires/malveillants
- Tentez buffer overflows, injections
- Loggez tous les crashes
- Utilisez hypothesis ou atheris

Cibles : Tous vos endpoints d'API
```

---

## 3. Injections SQL et NoSQL

### 🟢 Niveau Facile

**Exercice 3.1 : Détection d'Injection**
```
Identifiez la vulnérabilité :
query = "SELECT * FROM users WHERE username = '" + input + "'"

Expliquez :
1. Quel input exploite cette faille ?
2. Quel est l'impact ?
3. Comment la corriger ?
```

**Exercice 3.2 : Prepared Statement Simple**
```
Réécrivez avec parameterized query :
# Vulnérable
cursor.execute(f"SELECT * FROM products WHERE id = {product_id}")

# Sécurisé
cursor.execute(???)

Testez avec product_id = "1 OR 1=1"
```

**Exercice 3.3 : Login Sécurisé**
```
Fonction secure_login(username, password) :
- Utilisez parameterized queries
- Comparez hash du password
- Gérez les erreurs sans leak d'info

Bloquez : admin' OR '1'='1
```

### 🟡 Niveau Intermédiaire

**Exercice 3.4 : ORM (SQLAlchemy)**
```
Refactorisez en utilisant un ORM :
- Définissez modèles User, Product
- Requêtes avec .filter() au lieu de SQL brut
- Relations (User.orders)
- Migrations avec Alembic

Avantage : Protection automatique contre injections
```

**Exercice 3.5 : NoSQL Injection (MongoDB)**
```
Identifiez et corrigez :
db.users.find({ "username": req.body.username })

Exploit : {"$ne": null} pour bypass
Solution : Validation stricte des types
```

**Exercice 3.6 : Stored Procedures**
```
Créez une stored procedure SQL :
CREATE PROCEDURE GetUserOrders(IN user_id INT)
BEGIN
  SELECT * FROM orders WHERE user_id = user_id;
END;

Appelez depuis Python de manière sécurisée
```

**Exercice 3.7 : Dynamic Query Builder**
```
Fonction build_search(filters) sécurisée :
- Accepte dict de filtres {"price_min": 10, "category": "books"}
- Construit WHERE clause dynamique
- Utilise parameterized queries
- Whitelist des colonnes autorisées

Challenge : Supportez AND, OR, comparateurs
```

### 🔴 Niveau Avancé

**Exercice 3.8 : Blind SQL Injection**
```
Simulez et défendez contre blind injection :
1. Créez un endpoint vulnérable (timing-based)
2. Script d'exploit qui extrait DB name
3. Implémentez WAF qui détecte les patterns
4. Testez avec sqlmap

Protection : Rate limiting + anomaly detection
```

**Exercice 3.9 : GraphQL Injection**
```
Sécurisez une API GraphQL :
- Validez depth des queries (<5 niveaux)
- Limitez complexity (max 1000)
- Whitelist des champs accessible
- Désactivez introspection en prod

Test : Query récursive infinie
```

**Exercice 3.10 : Audit de Code Automatisé**
```
Créez un scanner de vulnérabilités SQL :
- Parse code Python avec AST
- Détectez f-strings dans queries
- Identifiez .execute() sans paramètres
- Rapport avec ligne et sévérité
- Intégrez dans CI/CD

Bonus : Support de plusieurs langages
```

---

## 4. XSS (Cross-Site Scripting)

### 🟢 Niveau Facile

**Exercice 4.1 : Reflected XSS**
```
Identifiez la vulnérabilité :
<h1>Bienvenue <?= $_GET['name'] ?></h1>

1. Quel input injecte du JS ?
2. Créez un payload qui affiche alert()
3. Corrigez avec htmlspecialchars()
```

**Exercice 4.2 : Template Escaping**
```
Utilisez Jinja2 avec auto-escaping :
# Vulnérable
{{ user_comment }}

# Sécurisé
{{ user_comment | e }}

Testez : "<script>alert('XSS')</script>"
```

**Exercice 4.3 : Sanitization Basique**
```
Fonction sanitize_html(text) :
- Remplace < par &lt;
- Remplace > par &gt;
- Remplace " par &quot;
- Garde le texte lisible

Utilisez sur tous les outputs utilisateur
```

### 🟡 Niveau Intermédiaire

**Exercice 4.4 : Stored XSS**
```
Scénario blog avec commentaires :
1. Utilisateur poste commentaire malveillant
2. Stocké en DB sans sanitisation
3. Affiché à tous les visiteurs
4. JS s'exécute chez les victimes

Tâches :
- Créez le blog vulnérable
- Exploitez avec cookie stealing
- Corrigez avec DOMPurify ou bleach
```

**Exercice 4.5 : DOM-based XSS**
```
Trouvez et corrigez :
<script>
  const name = location.hash.substring(1);
  document.write("Hello " + name);
</script>

Exploit : #<img src=x onerror=alert()>
Solution : Utilisez textContent au lieu de innerHTML
```

**Exercice 4.6 : Whitelist HTML**
```
Fonction allow_safe_html(html) :
- Autorisez uniquement : <p>, <b>, <i>, <a>
- Attributs autorisés : href (URLs valides seulement)
- Supprimez tout le reste
- Utilisez bibliothèque comme bleach

Test : Payload complexe avec multiple encodings
```

**Exercice 4.7 : Content Security Policy**
```
Configurez CSP header qui :
- Bloque inline scripts
- Autorise scripts de votre domaine
- Permet images de CDN
- Rapport violations

Header :
Content-Security-Policy: script-src 'self'; ...

Validez avec csp-evaluator.withgoogle.com
```

### 🔴 Niveau Avancé

**Exercice 4.8 : XSS via SVG Upload**
```
Protégez contre XSS dans uploads SVG :
1. SVG peut contenir <script>
2. Validez et sanitisez le XML
3. Servez avec Content-Type correct
4. Considérez conversion en PNG

Testez : Upload SVG avec payload
```

**Exercice 4.9 : Mutation XSS (mXSS)**
```
Défendez contre mXSS :
- Sanitisation bypassed par mutation du DOM
- Exemple : <noscript><p title="</noscript><img src=x onerror=alert()>">

Solutions :
- Double sanitisation
- Parseur HTML strict
- Test avec DOMPurify
```

**Exercice 4.10 : XSS Hunter**
```
Créez un honeypot pour détecter XSS :
1. Injectez canary tokens dans pages
2. Si JS exécuté, envoie beacon à votre serveur
3. Loggez : URL, payload, victime IP
4. Alertes en temps réel

Architecture : Serveur de collecte + dashboard
```

---

## 5. CSRF (Cross-Site Request Forgery)

### 🟢 Niveau Facile

**Exercice 5.1 : Comprendre CSRF**
```
Créez un site vulnérable :
- Formulaire de transfert d'argent (POST /transfer)
- Pas de protection CSRF
- Cookie de session valide

Site attaquant :
- Formulaire caché qui auto-submit
- Démontre l'attaque
```

**Exercice 5.2 : Token CSRF Simple**
```
Implémentez protection basique :
1. Générez token aléatoire dans session
2. Injectez dans formulaire (<input hidden>)
3. Validez token côté serveur
4. Rejetez si mismatch

Testez : Formulaire sans token
```

**Exercice 5.3 : Double Submit Cookie**
```
Pattern double submit :
- Token dans cookie
- Même token dans formulaire
- Serveur compare les deux
- Pas besoin de stockage session

Implémentez et testez
```

### 🟡 Niveau Intermédiaire

**Exercice 5.4 : CSRF avec AJAX**
```
Protégez API REST :
- Token dans header X-CSRF-Token
- Généré au chargement de la page
- Stocké dans meta tag
- Envoyé avec fetch()

Exemple :
fetch('/api/update', {
  headers: { 'X-CSRF-Token': token }
})
```

**Exercice 5.5 : SameSite Cookie**
```
Configurez cookies de session :
Set-Cookie: session=...; SameSite=Strict; Secure; HttpOnly

Testez les différences :
- None : Vulnérable CSRF
- Lax : Protection partielle
- Strict : Protection complète

Vérifiez compatibilité navigateurs
```

**Exercice 5.6 : Origin Header Validation**
```
Validez Origin/Referer :
def validate_origin(request):
  origin = request.headers.get('Origin')
  if origin not in ALLOWED_ORIGINS:
    return 403
  ...

Attention : Headers peuvent être absents
Combinez avec token CSRF
```

**Exercice 5.7 : Framework Integration**
```
Utilisez CSRF protection native :
- Django : {% csrf_token %}
- Flask : CSRFProtect
- Express : csurf middleware

Configurez pour toute l'app
Exemptez endpoints publics (ex: webhooks)
```

### 🔴 Niveau Avancé

**Exercice 5.8 : CSRF Login**
```
Protégez contre login CSRF :
- Attaquant force login avec son compte
- Victime pense être sur son compte
- Sensible data leakée

Solutions :
- Token sur login aussi
- Détection de changement de session
```

**Exercice 5.9 : CSRF Chain Attack**
```
Scénario complexe :
1. CSRF change email
2. CSRF change password (envoyé au nouvel email)
3. Account takeover complet

Défenses multi-couches :
- Re-authentication pour actions sensibles
- Email de confirmation
- Rate limiting
```

**Exercice 5.10 : WebSocket CSRF**
```
Sécurisez WebSocket contre CSRF :
- ws:// n'a pas de same-origin policy
- Validez Origin header
- Token dans premier message
- Challenge-response handshake

Implémentez serveur WS sécurisé
```

---

## 6. Cryptographie et Hachage

### 🟢 Niveau Facile

**Exercice 6.1 : Hash MD5 (Ne pas utiliser en prod)**
```
Démontrez faiblesse de MD5 :
- Hachez un password avec MD5
- Cherchez sur CrackStation
- Crackez avec hashcat

Conclusion : MD5 est cassé, utilisez bcrypt
```

**Exercice 6.2 : Bcrypt Basique**
```
Fonction hash_password(password) :
- Utilisez bcrypt avec cost factor 12
- Fonction verify_password(password, hash)
- Testez temps de hachage

Benchmark : Doit prendre ~100-300ms
```

**Exercice 6.3 : Salt Automatique**
```
Comprenez le salting :
1. Hachez "password123" deux fois avec bcrypt
2. Observez hashes différents
3. Expliquez pourquoi
4. Pourquoi salt aléatoire est important ?

Réponse : Rainbow tables inutiles
```

### 🟡 Niveau Intermédiaire

**Exercice 6.4 : Argon2 Winner**
```
Implémentez Argon2id (winner PHC 2015) :
- Argon2id pour passwords
- Paramètres : memory=64MB, iterations=3
- Comparez performances vs bcrypt
- Migration de bcrypt vers Argon2

Librairie : argon2-cffi
```

**Exercice 6.5 : Encryption Symétrique**
```
Chiffrez des données sensibles :
- AES-256-GCM
- Clé dérivée avec PBKDF2
- Stockez IV avec ciphertext
- Fonction encrypt_data(plaintext, key)
- Fonction decrypt_data(ciphertext, key)

Utilisez cryptography library
```

**Exercice 6.6 : Gestion de Clés**
```
Key rotation strategy :
1. Clés identifiées par version
2. Stockage sécurisé (environment variables)
3. Fonction reencrypt_with_new_key()
4. Old keys gardées pour déchiffrement

Schema DB :
encrypted_data | key_version | iv
```

**Exercice 6.7 : Password Reset Token**
```
Token de reset sécurisé :
- Générez avec secrets.token_urlsafe(32)
- Hachez avant stockage en DB
- Expiration 1 heure
- One-time use (supprimé après usage)
- Associé à user_id et timestamp

Workflow complet
```

### 🔴 Niveau Avancé

**Exercice 6.8 : RSA Key Pair**
```
Implémentez cryptographie asymétrique :
1. Générez paire de clés RSA-4096
2. Chiffrez message avec clé publique
3. Déchiffrez avec clé privée
4. Signature numérique de document
5. Vérification de signature

Use case : API inter-services
```

**Exercice 6.9 : Perfect Forward Secrecy**
```
Implémentez Diffie-Hellman key exchange :
- Deux parties génèrent ephemeral keys
- Échangent publiques
- Calculent shared secret
- Utilisent pour chiffrer conversation
- Clés détruites après session

Simule TLS handshake
```

**Exercice 6.10 : Hardware Security Module**
```
Intégrez HSM ou Key Management Service :
- AWS KMS ou HashiCorp Vault
- Clés jamais exposées en plaintext
- API pour encrypt/decrypt
- Audit logs des usages
- Key rotation automatique

Architecture cloud-native
```

---

## 7. Gestion des Permissions et Autorisations

### 🟢 Niveau Facile

**Exercice 7.1 : Rôles Simples**
```
Implémentez 3 rôles :
- guest : lecture seule
- user : lecture + création
- admin : tous droits

Fonction can_access(user, action) :
if user.role == "admin":
  return True
...
```

**Exercice 7.2 : Decorator de Permissions**
```
Créez decorator @require_role("admin") :
def require_role(role):
  def decorator(func):
    def wrapper(user, *args):
      if user.role != role:
        raise PermissionError
      return func(user, *args)
    return wrapper
  return decorator

@require_role("admin")
def delete_user(user, target_id):
  ...
```

**Exercice 7.3 : Propriété des Ressources**
```
Vérifiez ownership :
def can_edit_post(user, post):
  return post.author_id == user.id or user.is_admin

Appliquez sur tous les endpoints de modification
```

### 🟡 Niveau Intermédiaire

**Exercice 7.4 : RBAC (Role-Based Access Control)**
```
Système RBAC complet :

Tables :
- roles (id, name)
- permissions (id, resource, action)
- role_permissions (role_id, permission_id)
- user_roles (user_id, role_id)

Fonction : user.has_permission("posts:delete")
```

**Exercice 7.5 : ACL (Access Control List)**
```
ACL par ressource :
document.acl = {
  "user:123": ["read", "write"],
  "group:editors": ["read"],
  "public": ["read"]
}

Fonction : check_acl(user, document, action)
Héritage de permissions
```

**Exercice 7.6 : Permissions Hiérarchiques**
```
Tree de permissions :
admin
├── users
│   ├── create
│   ├── edit
│   └── delete
└── content
    ├── publish
    └── moderate

admin:users implique admin:users:create, edit, delete
```

**Exercice 7.7 : Context-Based Permissions**
```
Permissions dépendent du contexte :
def can_edit_post(user, post, time):
  # Auteur peut éditer dans 24h
  if user.id == post.author_id:
    return time - post.created_at < 24*3600
  # Modérateurs toujours
  return user.has_role("moderator")

Gérez edge cases
```

### 🔴 Niveau Avancé

**Exercice 7.8 : ABAC (Attribute-Based Access Control)**
```
Policies basées sur attributs :
{
  "effect": "allow",
  "principal": {"department": "HR"},
  "action": ["read", "write"],
  "resource": {"type": "employee_record"},
  "condition": {
    "employee.department": "${user.department}"
  }
}

Moteur d'évaluation de policies
```

**Exercice 7.9 : OAuth 2.0 Scopes**
```
Implémentez system de scopes :
- read:profile
- write:profile
- read:posts
- admin:users

Token JWT contient scopes
Middleware vérifie scope requis par endpoint
Granularité fine des permissions API
```

**Exercice 7.10 : Policy as Code (OPA)**
```
Intégrez Open Policy Agent :
1. Policies en Rego language
2. Service OPA séparé
3. App query OPA pour décisions
4. Logs centralisés des autorisations
5. Tests unitaires des policies

Exemple policy :
allow {
  input.user.role == "admin"
}
```

---

## 8. Sécurité des APIs REST

### 🟢 Niveau Facile

**Exercice 8.1 : API Key Simple**
```
Protection par API key :
- Header : X-API-Key
- Vérifiez contre liste en DB
- Retournez 401 si invalide

Test : curl avec et sans key
```

**Exercice 8.2 : Rate Limiting Basique**
```
Limitez à 100 requêtes/heure par IP :
- Compteur en mémoire (dict)
- Resetté chaque heure
- HTTP 429 si dépassé
- Header : X-RateLimit-Remaining

Utilisez time.time() pour tracking
```

**Exercice 8.3 : CORS Configuration**
```
Configurez CORS headers :
Access-Control-Allow-Origin: https://frontend.com
Access-Control-Allow-Methods: GET, POST
Access-Control-Allow-Headers: Content-Type

Testez preflight OPTIONS request
```

### 🟡 Niveau Intermédiaire

**Exercice 8.4 : JWT Bearer Token**
```
Authentification par JWT :
- POST /login retourne JWT
- Endpoints protégés vérifient Bearer token
- Extraire user_id du JWT payload
- Vérifier expiration et signature

Authorization: Bearer eyJhbGc...
```

**Exercice 8.5 : API Versioning**
```
Versionnez votre API :
- /api/v1/users
- /api/v2/users (breaking changes)
- Header Accept: application/vnd.api+json;version=2
- Documentez migrations

Maintenez v1 pendant 6 mois
```

**Exercice 8.6 : Input Validation Middleware**
```
Middleware de validation automatique :
@app.route('/users', methods=['POST'])
@validate_schema({
  "username": {"type": "string", "minLength": 3},
  "email": {"type": "string", "format": "email"}
})
def create_user():
  ...

Retourne 400 avec erreurs détaillées
```

**Exercice 8.7 : Pagination Sécurisée**
```
Endpoint /posts avec pagination :
- Paramètres : page, limit (max 100)
- Validez page > 0
- Empêchez limit énorme (DOS)
- Links : next, prev, first, last
- Metadata : total_items, total_pages

Format : JSON API ou HAL
```

### 🔴 Niveau Avancé

**Exercice 8.8 : GraphQL Security**
```
Sécurisez API GraphQL :
1. Query depth limiting (max 5)
2. Query complexity analysis
3. Disable introspection en prod
4. Pagination forcée sur listes
5. Rate limiting par operation

Testez avec queries malicieuses
```

**Exercice 8.9 : API Gateway**
```
Implémentez API Gateway :
- Point d'entrée unique
- Authentification centralisée
- Rate limiting global
- Request/response logging
- Routing vers microservices
- Circuit breaker pattern

Stack : Kong, Tyk ou custom
```

**Exercice 8.10 : OWASP API Security**
```
Auditez contre OWASP API Top 10 :
1. Broken Object Level Authorization
2. Broken Authentication
3. Excessive Data Exposure
4. Lack of Resources & Rate Limiting
5. Broken Function Level Authorization
6. Mass Assignment
7. Security Misconfiguration
8. Injection
9. Improper Assets Management
10. Insufficient Logging & Monitoring

Checklist complète pour chaque endpoint
```

---

## 9. HTTPS et Certificats SSL/TLS

### 🟢 Niveau Facile

**Exercice 9.1 : Certificat Auto-Signé**
```
Générez certificat SSL self-signed :
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365

Configurez serveur Flask/Express avec HTTPS
Accédez via https://localhost:5000
Observez warning du navigateur
```

**Exercice 9.2 : Let's Encrypt**
```
Obtenez certificat gratuit :
1. Installez certbot
2. Domaine pointant vers votre serveur
3. certbot --nginx -d example.com
4. Auto-renouvellement configuré

Vérifiez avec ssllabs.com
```

**Exercice 9.3 : Redirect HTTP vers HTTPS**
```
Configuration nginx/Apache :
server {
  listen 80;
  return 301 https://$host$request_uri;
}

Testez : curl -I http://example.com
Expect : 301 Location: https://...
```

### 🟡 Niveau Intermédiaire

**Exercice 9.4 : HSTS Header**
```
HTTP Strict Transport Security :
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload

- Force HTTPS pendant 1 an
- Inclus sous-domaines
- Submittez à HSTS preload list

Test : Tentez HTTP après première visite
```

**Exercice 9.5 : Certificate Pinning**
```
Implémentez pinning dans app mobile :
1. Extrayez hash du certificat serveur
2. Hardcodez dans app
3. Vérifiez pendant handshake
4. Rejetez si mismatch

Protection contre MITM même avec CA compromise
```

**Exercice 9.6 : TLS Configuration**
```
Durcissez config TLS :
- Désactivez TLS 1.0, 1.1
- TLS 1.2+ uniquement
- Cipher suites modernes (ECDHE, AES-GCM)
- Préférence serveur
- Session tickets off

OpenSSL config ou nginx ssl_ciphers
```

**Exercice 9.7 : Monitoring Expiration**
```
Alertes certificat expirant :
- Script vérifie expiration
- Alerte si < 30 jours
- Cron journalier
- Notifications email/Slack

openssl s_client -connect example.com:443 | openssl x509 -noout -dates
```

### 🔴 Niveau Avancé

**Exercice 9.8 : mTLS (Mutual TLS)**
```
Authentification bidirectionnelle :
1. Serveur a certificat
2. Clients ont aussi certificats
3. Handshake vérifie les deux
4. CA privée pour clients

Use case : API inter-services
Nginx : ssl_verify_client on
```

**Exercice 9.9 : Certificate Transparency**
```
Implémentez CT monitoring :
1. Surveillez CT logs (crt.sh)
2. Détectez certificats émis pour vos domaines
3. Alertes si certificat non-autorisé
4. Expect-CT header

Protection contre mis-issuance
```

**Exercice 9.10 : Zero-Trust Network**
```
Architecture zero-trust :
- Tous services communiquent en mTLS
- Service mesh (Istio, Linkerd)
- Policies par service
- Rotation automatique des certs
- No trust by default

Déployez microservices avec mTLS
```

---

## 10. Logging et Monitoring de Sécurité

### 🟢 Niveau Facile

**Exercice 10.1 : Logging Basique**
```
Loggez événements de sécurité :
- Logins réussis/échoués
- Changements de password
- Accès refusés (403)
- Timestamp, user_id, IP, action

Format : JSON pour parsing facile
```

**Exercice 10.2 : Fichiers de Logs**
```
Configuration logging :
- logs/security.log
- Rotation quotidienne
- Retention 90 jours
- Permissions 600 (read owner only)

Python logging.handlers.RotatingFileHandler
```

**Exercice 10.3 : Détection Brute Force**
```
Détectez tentatives de brute force :
- Comptez logins échoués par IP
- Si > 10 en 5 minutes, alerte
- Log IP, count, timeframe

Script d'analyse des logs
```

### 🟡 Niveau Intermédiaire

**Exercice 10.4 : Centralized Logging**
```
Stack ELK (Elasticsearch, Logstash, Kibana) :
1. Apps envoient logs à Logstash
2. Indexés dans Elasticsearch
3. Visualisés dans Kibana
4. Dashboards : Failed logins, API errors, etc.

Ou alternative : Splunk, Datadog
```

**Exercice 10.5 : Structured Logging**
```
Logs structurés JSON :
{
  "timestamp": "2025-11-28T10:00:00Z",
  "level": "WARNING",
  "event": "login_failed",
  "user": "john@example.com",
  "ip": "192.168.1.100",
  "user_agent": "Mozilla...",
  "reason": "invalid_password"
}

Facilite queries et analyses
```

**Exercice 10.6 : Alertes Automatiques**
```
Système d'alerting :
- Règles : 5+ failed logins en 1 min
- 403 errors spike
- Admin privilege escalation
- Notifications : Email, Slack, PagerDuty
- Severité : info, warning, critical

Implémentez avec rules engine
```

**Exercice 10.7 : Audit Trail**
```
Traçabilité complète :
- Table audit_logs
- Colonnes : user_id, action, resource_type, resource_id, changes (JSON), timestamp
- Trigger sur UPDATE/DELETE
- Immutable (append-only)
- Retention légale (7 ans RGPD)

Queries : Who modified record X when?
```

### 🔴 Niveau Avancé

**Exercice 10.8 : SIEM Integration**
```
Security Information and Event Management :
1. Agrégez logs de toutes sources
2. Corrélation d'événements
3. Détection d'anomalies ML
4. Playbooks de réponse automatique
5. Dashboards compliance (PCI-DSS, SOC2)

Stack : Wazuh, AlienVault, QRadar
```

**Exercice 10.9 : Intrusion Detection**
```
IDS avec Snort ou Suricata :
- Analyse trafic réseau
- Signatures d'attaques
- Alertes en temps réel
- Intégration avec firewall (IPS)
- Règles custom pour votre app

Testez avec Metasploit
```

**Exercice 10.10 : Incident Response**
```
Workflow de réponse aux incidents :
1. Detection (SIEM alerte)
2. Containment (isoler système compromis)
3. Investigation (forensics sur logs)
4. Eradication (supprimer malware)
5. Recovery (restaurer services)
6. Lessons learned (post-mortem)

Créez playbook et testez avec tabletop exercise
```

---

## 🎯 Projets Finaux Intégrateurs

### Projet 1 : E-commerce Sécurisé
```
Application complète avec :
- Authentification (JWT + MFA)
- Paiements (PCI-DSS compliance)
- Panel admin (RBAC)
- API REST sécurisée
- Logging SIEM
- Pentesting report

Stack suggéré : Django + PostgreSQL + Redis
```

### Projet 2 : Plateforme de Partage de Fichiers
```
Features :
- Upload sécurisé (virus scan)
- Encryption at rest
- Partage avec permissions granulaires
- Audit trail
- HTTPS strict
- Rate limiting

Similaire à Dropbox/Google Drive
```

### Projet 3 : API Banking
```
Haute sécurité :
- mTLS pour tous clients
- Transaction signing
- Fraud detection ML
- Immutable audit logs
- Zero-trust architecture
- Incident response plan

Conforme PSD2/Open Banking
```

---

## 📚 Ressources Complémentaires

**Standards et Best Practices :**
- OWASP Top 10
- NIST Cybersecurity Framework
- CWE/SANS Top 25

**Outils de Test :**
- Burp Suite / OWASP ZAP
- SQLMap
- Metasploit
- Nmap

**Certifications :**
- CEH (Certified Ethical Hacker)
- OSCP (Offensive Security)
- CISSP

**Labs et CTF :**
- HackTheBox
- TryHackMe
- PortSwigger Web Security Academy

---


Bon apprentissage !
By : Méryl BITEE