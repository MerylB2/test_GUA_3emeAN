# Guide de Suivi de Projet
## Conception et Développement d'Applications Sécurisées

---

## Table des Matières

1. [Introduction à Ubuntu](#1-introduction-à-ubuntu)
2. [Commandes Bash Essentielles](#2-commandes-bash-essentielles)
3. [Administration Système Linux](#3-administration-système-linux)
4. [Versionnage avec Git](#4-versionnage-avec-git)
5. [Utilisation de GitHub](#5-utilisation-de-github)
6. [Bonnes Pratiques pour Applications Sécurisées](#6-bonnes-pratiques-pour-applications-sécurisées)

---

## 1. Introduction à Ubuntu

### 1.1 Qu'est-ce qu'Ubuntu ?

Ubuntu est une distribution Linux basée sur Debian, reconnue pour sa stabilité et sa facilité d'utilisation. Elle est particulièrement adaptée au développement d'applications sécurisées grâce à son écosystème robuste et ses mises à jour de sécurité régulières.

### 1.2 Navigation de Base

#### Structure des Répertoires Linux
```
/           - Racine du système
/home       - Répertoires personnels des utilisateurs
/etc        - Fichiers de configuration système
/var        - Données variables (logs, cache)
/usr        - Applications et bibliothèques utilisateur
/tmp        - Fichiers temporaires
/opt        - Applications tierces
```

#### Commandes de Navigation
```bash
pwd                    # Affiche le répertoire courant
cd /chemin/vers/dir    # Change de répertoire
cd ~                   # Retourne au répertoire personnel
cd ..                  # Remonte d'un niveau
cd -                   # Retourne au répertoire précédent
ls                     # Liste les fichiers
ls -la                 # Liste détaillée incluant les fichiers cachés
```

---

## 2. Commandes Bash Essentielles

### 2.1 Gestion des Fichiers et Répertoires

#### Création et Suppression
```bash
# Créer un fichier vide
touch fichier.txt

# Créer un répertoire
mkdir mon_dossier
mkdir -p parent/enfant/petit-enfant    # Crée les parents si nécessaire

# Copier
cp source.txt destination.txt          # Copie un fichier
cp -r dossier/ nouveau_dossier/        # Copie récursive de répertoire

# Déplacer/Renommer
mv ancien.txt nouveau.txt              # Renomme un fichier
mv fichier.txt /autre/emplacement/     # Déplace un fichier

# Supprimer
rm fichier.txt                         # Supprime un fichier
rm -r dossier/                         # Supprime récursivement
rm -rf dossier/                        # Force la suppression (ATTENTION!)
```

### 2.2 Visualisation et Édition de Fichiers

```bash
# Afficher le contenu
cat fichier.txt                        # Affiche tout le contenu
less fichier.txt                       # Pagination (q pour quitter)
head -n 10 fichier.txt                 # 10 premières lignes
tail -n 10 fichier.txt                 # 10 dernières lignes
tail -f log.txt                        # Suivi en temps réel

# Recherche dans les fichiers
grep "motif" fichier.txt               # Recherche un motif
grep -r "motif" dossier/               # Recherche récursive
grep -i "motif" fichier.txt            # Insensible à la casse
grep -n "motif" fichier.txt            # Avec numéros de ligne

# Édition
nano fichier.txt                       # Éditeur simple (Ctrl+X pour quitter)
vim fichier.txt                        # Éditeur avancé
```

### 2.3 Gestion des Processus

```bash
# Lister les processus
ps aux                                 # Tous les processus
ps aux | grep nom_processus            # Recherche un processus spécifique
top                                    # Moniteur interactif
htop                                   # Version améliorée (si installé)

# Gestion des processus
kill PID                               # Termine un processus
kill -9 PID                            # Force l'arrêt
killall nom_processus                  # Termine par nom
bg                                     # Place en arrière-plan
fg                                     # Ramène en avant-plan
nohup commande &                       # Lance sans dépendre du terminal
```

### 2.4 Redirections et Pipes

```bash
# Redirections
commande > fichier.txt                 # Écrit (écrase)
commande >> fichier.txt                # Ajoute à la fin
commande 2> erreurs.log                # Redirige les erreurs
commande &> tout.log                   # Redirige tout

# Pipes (chaînage)
cat fichier.txt | grep "motif"         # Filtre le contenu
ps aux | grep python | wc -l           # Compte les processus Python
ls -l | sort -k5 -n                    # Trie par taille
```

### 2.5 Permissions et Propriétés

```bash
# Afficher les permissions
ls -l fichier.txt
# Exemple de sortie: -rw-r--r-- 1 user group 1234 Nov 14 10:00 fichier.txt
# Format: type | user | group | others

# Changer les permissions
chmod 755 script.sh                    # rwxr-xr-x
chmod +x script.sh                     # Ajoute l'exécution
chmod u+w,g-r fichier.txt              # Modifications spécifiques
chmod -R 644 dossier/                  # Récursif

# Changer le propriétaire
chown user:group fichier.txt           # Change propriétaire et groupe
chown -R user:group dossier/           # Récursif
```

### 2.6 Compression et Archives

```bash
# TAR
tar -cvf archive.tar dossier/          # Créer une archive
tar -xvf archive.tar                   # Extraire
tar -czvf archive.tar.gz dossier/      # Créer et compresser (gzip)
tar -xzvf archive.tar.gz               # Extraire archive compressée

# ZIP
zip -r archive.zip dossier/            # Créer un zip
unzip archive.zip                      # Extraire un zip
unzip -l archive.zip                   # Lister le contenu
```

### 2.7 Réseau et Connectivité

```bash
# Informations réseau
ip addr show                           # Affiche les interfaces réseau
ip route show                          # Affiche les routes
ping google.com                        # Test de connectivité
traceroute google.com                  # Trace la route
netstat -tuln                          # Ports en écoute
ss -tuln                               # Alternative moderne à netstat

# Téléchargement
wget https://example.com/file.zip      # Télécharge un fichier
curl https://api.example.com           # Requête HTTP
curl -X POST -d "data" url             # POST avec données
```

---

## 3. Administration Système Linux

### 3.1 Gestion des Utilisateurs

#### Commandes Utilisateur de Base
```bash
# Ajouter un utilisateur
sudo adduser nouveau_user              # Méthode interactive
sudo useradd -m -s /bin/bash user      # Méthode manuelle

# Modifier un utilisateur
sudo usermod -aG sudo username         # Ajoute aux sudoers
sudo usermod -aG docker username       # Ajoute au groupe docker
sudo usermod -l nouveau ancien         # Renomme l'utilisateur
sudo usermod -d /nouveau/home user     # Change le répertoire home

# Supprimer un utilisateur
sudo deluser username                  # Supprime l'utilisateur
sudo deluser --remove-home username    # Supprime avec le home
sudo userdel -r username               # Alternative

# Gestion des mots de passe
passwd                                 # Change son propre mot de passe
sudo passwd username                   # Change le mot de passe d'un user
sudo passwd -l username                # Verrouille un compte
sudo passwd -u username                # Déverrouille un compte

# Informations utilisateur
whoami                                 # Utilisateur courant
id                                     # UID, GID et groupes
w                                      # Qui est connecté
last                                   # Historique des connexions
```

### 3.2 Gestion des Groupes

```bash
# Créer un groupe
sudo groupadd nom_groupe

# Ajouter un utilisateur à un groupe
sudo usermod -aG nom_groupe username
sudo gpasswd -a username nom_groupe    # Alternative

# Retirer un utilisateur d'un groupe
sudo gpasswd -d username nom_groupe

# Lister les groupes
groups                                 # Groupes de l'utilisateur courant
groups username                        # Groupes d'un utilisateur
cat /etc/group                         # Tous les groupes du système

# Supprimer un groupe
sudo groupdel nom_groupe
```

### 3.3 Privilèges Sudo

#### Configuration de Sudo
```bash
# Éditer la configuration sudo (TOUJOURS utiliser visudo)
sudo visudo

# Exemples de règles dans /etc/sudoers:
# Donner tous les droits sudo
username ALL=(ALL:ALL) ALL

# Sudo sans mot de passe (à utiliser avec précaution)
username ALL=(ALL) NOPASSWD: ALL

# Limiter à certaines commandes
username ALL=(ALL) NOPASSWD: /usr/bin/systemctl, /usr/bin/apt

# Exécuter une commande avec sudo
sudo apt update                        # Exécute avec privilèges root
sudo -u autre_user commande            # Exécute en tant qu'autre user
sudo -i                                # Shell root interactif
sudo su -                              # Devenir root
```

### 3.4 Gestion des Services (systemd)

```bash
# Contrôle des services
sudo systemctl start nom_service       # Démarre un service
sudo systemctl stop nom_service        # Arrête un service
sudo systemctl restart nom_service     # Redémarre un service
sudo systemctl reload nom_service      # Recharge la config sans redémarrer
sudo systemctl enable nom_service      # Active au démarrage
sudo systemctl disable nom_service     # Désactive au démarrage

# Informations sur les services
systemctl status nom_service           # État d'un service
systemctl list-units --type=service    # Liste tous les services
systemctl list-unit-files              # Liste tous les services disponibles
systemctl is-active nom_service        # Vérifie si actif
systemctl is-enabled nom_service       # Vérifie si activé au boot

# Logs des services
journalctl -u nom_service              # Logs d'un service
journalctl -u nom_service -f           # Suivi en temps réel
journalctl -u nom_service --since today
journalctl -xe                         # Logs récents du système
```

### 3.5 Gestion du Système

```bash
# Redémarrage et arrêt
sudo reboot                            # Redémarre le système
sudo shutdown -h now                   # Arrête immédiatement
sudo shutdown -h +10                   # Arrête dans 10 minutes
sudo shutdown -r now                   # Redémarre immédiatement
sudo shutdown -c                       # Annule un shutdown programmé

# Informations système
uname -a                               # Informations du noyau
hostnamectl                            # Informations sur l'hôte
uptime                                 # Temps de fonctionnement
free -h                                # Mémoire disponible
df -h                                  # Espace disque
lsblk                                  # Blocs de périphériques
lscpu                                  # Informations CPU
```

### 3.6 Configuration Réseau et Hostname

```bash
# Changer le hostname
sudo hostnamectl set-hostname nouveau-nom
sudo nano /etc/hosts                   # Mettre à jour aussi ici

# Configuration réseau (netplan - Ubuntu moderne)
sudo nano /etc/netplan/01-netcfg.yaml
sudo netplan apply                     # Applique les changements

# Exemple de configuration netplan:
# network:
#   version: 2
#   ethernets:
#     eth0:
#       dhcp4: true
#       # ou configuration statique:
#       addresses: [192.168.1.100/24]
#       gateway4: 192.168.1.1
#       nameservers:
#         addresses: [8.8.8.8, 8.8.4.4]
```

### 3.7 Paramètres Kernel (sysctl)

```bash
# Afficher tous les paramètres
sysctl -a

# Afficher un paramètre spécifique
sysctl net.ipv4.ip_forward

# Modifier temporairement (jusqu'au redémarrage)
sudo sysctl -w net.ipv4.ip_forward=1

# Modifier définitivement
sudo nano /etc/sysctl.conf
# Ajouter: net.ipv4.ip_forward = 1
sudo sysctl -p                         # Recharge la configuration

# Paramètres utiles pour la sécurité:
# net.ipv4.tcp_syncookies = 1          # Protection SYN flood
# net.ipv4.conf.all.rp_filter = 1      # Protection spoofing
# net.ipv4.icmp_echo_ignore_all = 1    # Ignore les pings
```

### 3.8 Gestion des Paquets (APT)

```bash
# Mise à jour
sudo apt update                        # Met à jour la liste des paquets
sudo apt upgrade                       # Met à jour les paquets installés
sudo apt full-upgrade                  # Upgrade avec dépendances
sudo apt dist-upgrade                  # Mise à jour de distribution

# Installation et suppression
sudo apt install nom_paquet            # Installe un paquet
sudo apt install paquet1 paquet2       # Installe plusieurs paquets
sudo apt remove nom_paquet             # Supprime un paquet
sudo apt purge nom_paquet              # Supprime avec configuration
sudo apt autoremove                    # Supprime dépendances inutiles

# Recherche et information
apt search mot_cle                     # Recherche un paquet
apt show nom_paquet                    # Infos sur un paquet
apt list --installed                   # Paquets installés
apt list --upgradable                  # Paquets pouvant être mis à jour
```

### 3.9 Pare-feu (UFW)

```bash
# Activation/Désactivation
sudo ufw enable                        # Active le pare-feu
sudo ufw disable                       # Désactive le pare-feu
sudo ufw status                        # État du pare-feu
sudo ufw status verbose                # État détaillé

# Règles de base
sudo ufw allow 22                      # Autorise SSH
sudo ufw allow 80/tcp                  # Autorise HTTP
sudo ufw allow 443/tcp                 # Autorise HTTPS
sudo ufw allow from 192.168.1.0/24     # Autorise un sous-réseau
sudo ufw deny 23                       # Bloque Telnet

# Gestion des règles
sudo ufw delete allow 80               # Supprime une règle
sudo ufw reset                         # Réinitialise toutes les règles
sudo ufw default deny incoming         # Politique par défaut
sudo ufw default allow outgoing
```

---

## 4. Versionnage avec Git

### 4.1 Configuration Initiale de Git

```bash
# Configuration utilisateur (obligatoire)
git config --global user.name "Votre Nom"
git config --global user.email "votre.email@example.com"

# Configuration de l'éditeur par défaut
git config --global core.editor nano
git config --global core.editor vim

# Afficher la configuration
git config --list
git config user.name

# Créer des alias utiles
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.lg "log --oneline --graph --all"
```

### 4.2 Initialisation et Clonage

```bash
# Créer un nouveau dépôt
git init                               # Initialise dans le dossier courant
git init mon-projet                    # Crée et initialise un nouveau dossier

# Cloner un dépôt existant
git clone https://github.com/user/repo.git
git clone https://github.com/user/repo.git nouveau-nom
git clone git@github.com:user/repo.git # Clone via SSH
```

### 4.3 Workflow de Base

```bash
# Vérifier l'état
git status                             # État des fichiers
git status -s                          # Version courte

# Ajouter des fichiers au staging
git add fichier.txt                    # Ajoute un fichier
git add .                              # Ajoute tous les fichiers modifiés
git add *.js                           # Ajoute tous les fichiers .js
git add -p                             # Ajout interactif par chunk

# Retirer du staging
git reset fichier.txt                  # Retire du staging
git reset                              # Retire tout du staging

# Commiter les changements
git commit -m "Message de commit"      # Commit avec message
git commit -am "Message"               # Add + commit (fichiers tracés)
git commit --amend                     # Modifie le dernier commit
git commit --amend -m "Nouveau msg"    # Modifie le message du dernier commit

# Voir l'historique
git log                                # Historique complet
git log --oneline                      # Version condensée
git log --graph --oneline --all        # Avec graphe visuel
git log -n 5                           # 5 derniers commits
git log --author="Nom"                 # Commits d'un auteur
git log --since="2 weeks ago"          # Depuis une date
git log --grep="fix"                   # Recherche dans les messages
git log fichier.txt                    # Historique d'un fichier

# Voir les différences
git diff                               # Différences non stagées
git diff --staged                      # Différences stagées
git diff HEAD                          # Toutes les différences
git diff commit1 commit2               # Entre deux commits
git diff branche1 branche2             # Entre deux branches
```

### 4.4 Gestion des Branches

```bash
# Lister les branches
git branch                             # Branches locales
git branch -a                          # Toutes les branches (locales + distantes)
git branch -r                          # Branches distantes
git branch -v                          # Avec dernier commit

# Créer et changer de branche
git branch nouvelle-branche            # Crée une branche
git checkout nouvelle-branche          # Change de branche
git checkout -b nouvelle-branche       # Crée et change de branche
git switch nouvelle-branche            # Change de branche (moderne)
git switch -c nouvelle-branche         # Crée et change (moderne)

# Renommer et supprimer
git branch -m ancien nouveau           # Renomme une branche
git branch -d branche                  # Supprime (si fusionnée)
git branch -D branche                  # Force la suppression

# Fusionner des branches
git checkout main                      # Se place sur la branche cible
git merge feature-branche              # Fusionne feature-branche
git merge --no-ff feature-branche      # Force un commit de merge
git merge --squash feature-branche     # Fusionne en un seul commit

# Résolution de conflits
# 1. Git marque les conflits dans les fichiers
# 2. Éditer les fichiers pour résoudre
# 3. git add fichiers-resolus
# 4. git commit
```

### 4.5 Rebase

```bash
# Rebaser sur une autre branche
git checkout feature-branche
git rebase main                        # Rebase sur main

# Rebase interactif (réorganiser l'historique)
git rebase -i HEAD~3                   # 3 derniers commits
# Options dans l'éditeur:
# pick   = utiliser le commit
# reword = utiliser mais changer le message
# edit   = utiliser et s'arrêter pour modifier
# squash = fusionner avec le commit précédent
# drop   = supprimer le commit

# Continuer/Annuler un rebase
git rebase --continue                  # Continue après résolution
git rebase --skip                      # Ignore le commit courant
git rebase --abort                     # Annule le rebase

# ATTENTION: Ne jamais rebaser des commits publics/poussés!
```

### 4.6 Manipulation de l'Historique

```bash
# Annuler des changements
git checkout -- fichier.txt            # Annule modifs d'un fichier
git restore fichier.txt                # Alternative moderne
git restore --staged fichier.txt       # Retire du staging

# Reset
git reset --soft HEAD~1                # Annule commit, garde staging
git reset --mixed HEAD~1               # Annule commit et staging (défaut)
git reset --hard HEAD~1                # ATTENTION: supprime tout
git reset --hard origin/main           # Réaligne sur distant

# Revert (crée un nouveau commit d'annulation)
git revert HEAD                        # Annule le dernier commit
git revert abc123                      # Annule un commit spécifique
git revert HEAD~3..HEAD                # Annule plusieurs commits

# Cherry-pick (appliquer un commit spécifique)
git cherry-pick abc123                 # Applique le commit abc123
git cherry-pick abc123 def456          # Applique plusieurs commits
```

### 4.7 Stash (Mise de côté)

```bash
# Mettre de côté des changements
git stash                              # Met de côté les modifs
git stash save "Description"           # Avec description
git stash -u                           # Inclut fichiers non tracés
git stash -a                           # Inclut tout (même .gitignore)

# Lister et appliquer
git stash list                         # Liste les stashs
git stash show                         # Montre le dernier stash
git stash show stash@{0}               # Montre un stash spécifique
git stash apply                        # Applique le dernier stash
git stash apply stash@{1}              # Applique un stash spécifique
git stash pop                          # Applique et supprime
git stash drop stash@{0}               # Supprime un stash
git stash clear                        # Supprime tous les stashs
```

### 4.8 Tags

```bash
# Créer des tags
git tag v1.0.0                         # Tag léger
git tag -a v1.0.0 -m "Version 1.0"     # Tag annoté
git tag -a v1.0.0 abc123               # Tag sur commit spécifique

# Lister et afficher
git tag                                # Liste tous les tags
git tag -l "v1.*"                      # Filtre les tags
git show v1.0.0                        # Détails d'un tag

# Pousser des tags
git push origin v1.0.0                 # Pousse un tag
git push origin --tags                 # Pousse tous les tags

# Supprimer des tags
git tag -d v1.0.0                      # Supprime localement
git push origin --delete v1.0.0        # Supprime sur distant
```

### 4.9 Remote (Dépôts Distants)

```bash
# Gérer les remotes
git remote                             # Liste les remotes
git remote -v                          # Avec URLs
git remote add origin url              # Ajoute un remote
git remote add upstream url            # Ajoute upstream (fork)
git remote rename origin nouveau-nom   # Renomme
git remote remove origin               # Supprime
git remote set-url origin nouvelle-url # Change l'URL

# Récupérer depuis distant
git fetch origin                       # Récupère sans fusionner
git fetch --all                        # Récupère tous les remotes
git pull origin main                   # Fetch + merge
git pull --rebase origin main          # Fetch + rebase

# Pousser vers distant
git push origin main                   # Pousse vers main
git push -u origin main                # Pousse et configure upstream
git push origin --delete branche       # Supprime branche distante
git push --force                       # Force push (DANGER!)
git push --force-with-lease            # Force push plus sûr
```

### 4.10 .gitignore

#### Comprendre .gitignore

Le fichier `.gitignore` indique à Git quels fichiers ou dossiers **ne doivent pas être versionnés**. C'est crucial pour :
- **Éviter les secrets** : mots de passe, clés API, tokens
- **Exclure les fichiers générés** : binaires, logs, cache
- **Garder le repo propre** : dépendances, fichiers système
- **Réduire la taille** : éviter fichiers volumineux inutiles

#### Structure et Syntaxe

```bash
# Créer un .gitignore à la racine du projet
nano .gitignore

# Syntaxe de base :
fichier.txt              # Ignore fichier.txt dans tous les dossiers
/fichier.txt             # Ignore fichier.txt à la racine uniquement
dossier/                 # Ignore tout le dossier
*.log                    # Ignore tous les .log
*.log.*                  # Ignore log.txt.1, error.log.old, etc.

# Patterns avancés :
**/*.pyc                 # *.pyc dans tous les sous-dossiers
**/node_modules/         # node_modules n'importe où
build/**/output.txt      # output.txt dans tous les sous-dossiers de build

# Négation (exceptions) :
*.log                    # Ignore tous les .log
!important.log           # SAUF important.log
!logs/critical/*.log     # SAUF les .log critiques

# Commentaires :
# Ceci est un commentaire
## Double # pour clarté visuelle

# Échapper les caractères spéciaux :
\#fichier.txt            # Fichier qui commence par #
\!important.txt          # Fichier qui commence par !
```

#### .gitignore Global (pour tous vos projets)

```bash
# Créer un .gitignore global
nano ~/.gitignore_global

# Y ajouter :
# Fichiers système
.DS_Store                # macOS
Thumbs.db                # Windows
Desktop.ini              # Windows
.Spotlight-V100          # macOS
.Trashes                 # macOS

# Éditeurs et IDEs
.vscode/
.idea/
*.swp                    # Vim
*.swo                    # Vim
*~                       # Emacs
.project                 # Eclipse
.settings/               # Eclipse

# Configurer Git pour l'utiliser
git config --global core.excludesfile ~/.gitignore_global
```

#### Templates par Langage/Framework

#### Python
```gitignore
# Fichier .gitignore pour Python

# Byte-compiled / optimized / DLL files
__pycache__/
*.py[cod]
*$py.class

# C extensions
*.so

# Distribution / packaging
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
share/python-wheels/
*.egg-info/
.installed.cfg
*.egg
MANIFEST

# Virtual environments
venv/
env/
ENV/
.venv
.virtualenv

# PyInstaller
*.manifest
*.spec

# Unit test / coverage
htmlcov/
.tox/
.nox/
.coverage
.coverage.*
.cache
nosetests.xml
coverage.xml
*.cover
*.py,cover
.hypothesis/
.pytest_cache/
cover/

# Jupyter Notebook
.ipynb_checkpoints
*.ipynb_checkpoints/

# IPython
profile_default/
ipython_config.py

# Django
*.log
local_settings.py
db.sqlite3
db.sqlite3-journal
media/
staticfiles/

# Flask
instance/
.webassets-cache

# Scrapy
.scrapy

# Sphinx documentation
docs/_build/

# PyBuilder
.pybuilder/
target/

# Celery
celerybeat-schedule
celerybeat.pid

# SageMath
*.sage.py

# Environments
.env
.env.local
.venv
env/
venv/
ENV/
env.bak/
venv.bak/

# mypy
.mypy_cache/
.dmypy.json
dmypy.json

# Pyre
.pyre/

# pytype
.pytype/

# Cython
cython_debug/
```

#### Node.js / JavaScript
```gitignore
# Fichier .gitignore pour Node.js

# Dependencies
node_modules/
jspm_packages/
bower_components/

# Production builds
dist/
build/
out/
.next/
.nuxt/

# Testing
coverage/
.nyc_output

# Environment variables
.env
.env.local
.env.development.local
.env.test.local
.env.production.local

# Logs
logs/
*.log
npm-debug.log*
yarn-debug.log*
yarn-error.log*
lerna-debug.log*
pnpm-debug.log*

# OS files
.DS_Store
*.swp
*.swo

# IDE
.vscode/
.idea/
*.sublime-project
*.sublime-workspace

# TypeScript
*.tsbuildinfo
dist-types/

# Package managers
.yarn/
.pnp.*
.yarn-integrity
package-lock.json  # Si vous utilisez yarn
yarn.lock          # Si vous utilisez npm

# Cache
.cache/
.parcel-cache/
.eslintcache
.stylelintcache

# Temporary files
tmp/
temp/

# Config privées
config.local.js
secrets.json
```

#### Java
```gitignore
# Fichier .gitignore pour Java

# Compiled class files
*.class

# Package Files
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar

# Maven
target/
pom.xml.tag
pom.xml.releaseBackup
pom.xml.versionsBackup
pom.xml.next
release.properties
dependency-reduced-pom.xml
buildNumber.properties
.mvn/timing.properties
.mvn/wrapper/maven-wrapper.jar

# Gradle
.gradle/
build/
!gradle-wrapper.jar
!**/src/main/**/build/
!**/src/test/**/build/

# IntelliJ IDEA
.idea/
*.iws
*.iml
*.ipr
out/

# Eclipse
.classpath
.project
.settings/
bin/

# NetBeans
nbproject/private/
nbbuild/
dist/
nbdist/
.nb-gradle/

# BlueJ
*.ctxt

# Mobile Tools for Java (J2ME)
.mtj.tmp/

# Virtual machine crash logs
hs_err_pid*
replay_pid*

# Log files
*.log
logs/
```

#### Docker
```gitignore
# Fichier .gitignore pour Docker

# Docker volumes
volumes/
data/

# Environment files
.env
.env.local
docker-compose.override.yml

# Logs
logs/
*.log
```

#### Templates Généraux par Cas d'Usage

#### Secrets et Configuration
```gitignore
# Secrets et credentials (CRITIQUE)
.env
.env.*
!.env.example
*.pem
*.key
*.cert
*.crt
*.p12
*.pfx
secrets.yml
config.local.*
credentials.json
auth.json
.secrets
.password
id_rsa*
*.keystore

# Tokens et API keys
.npmrc
.pypirc
```

#### Fichiers Système et IDE
```gitignore
# macOS
.DS_Store
.AppleDouble
.LSOverride
._*
.Spotlight-V100
.Trashes

# Windows
Thumbs.db
ehthumbs.db
Desktop.ini
$RECYCLE.BIN/
*.lnk

# Linux
.directory
.Trash-*

# IDEs et éditeurs
.vscode/
.idea/
*.swp
*.swo
*~
.project
.settings/
.classpath
*.sublime-project
*.sublime-workspace
```

#### Fichiers de Build et Cache
```gitignore
# Build outputs
build/
dist/
out/
target/
*.o
*.pyc
*.class
*.dll
*.exe
*.so
*.dylib

# Cache
.cache/
*.cache
.temp/
tmp/
temp/
```

#### Utilisation Avancée

#### Vérifier ce qui est Ignoré
```bash
# Voir tous les fichiers ignorés
git status --ignored

# Vérifier si un fichier spécifique est ignoré
git check-ignore -v fichier.txt
# Sortie : .gitignore:10:*.txt    fichier.txt

# Lister tous les fichiers ignorés
git ls-files --others --ignored --exclude-standard

# Déboguer les règles .gitignore
git check-ignore -v **/*
```

#### Ignorer un Fichier Déjà Tracké
```bash
# Problème : Fichier déjà commité mais maintenant dans .gitignore
# Il faut le retirer de l'index Git

# Retirer du tracking (garde le fichier local)
git rm --cached fichier.txt
git rm -r --cached dossier/

# Ajouter au .gitignore
echo "fichier.txt" >> .gitignore

# Commiter
git add .gitignore
git commit -m "chore: stop tracking fichier.txt"
```

#### .gitignore par Dossier
```bash
# Vous pouvez avoir plusieurs .gitignore dans différents dossiers
projet/
├── .gitignore              # Règles globales du projet
├── src/
│   └── .gitignore          # Règles spécifiques à src/
└── tests/
    └── .gitignore          # Règles spécifiques à tests/

# Les règles des sous-dossiers s'ajoutent aux règles parentes
```

#### Patterns Avancés et Cas Spéciaux

```bash
# Ignorer tous les .log SAUF dans un dossier spécifique
*.log
!logs/keep/*.log

# Ignorer tous les fichiers dans un dossier SAUF certains
build/*
!build/.gitkeep
!build/README.md

# Ignorer les fichiers avec double extension
*.min.js
*.bundle.js

# Ignorer les fichiers numériques
backup.[0-9]
log.[0-9][0-9]

# Ignorer basé sur des patterns complexes
**/test_*.py              # Tous les test_*.py partout
src/**/build/             # Tous les build/ dans src/
**/migrations/*.py        # Migrations Python n'importe où
!**/migrations/__init__.py # Sauf __init__.py

# Caractère d'échappement
\#hashtag.txt             # Fichier commençant par #
```

#### Générer des .gitignore

```bash
# Utiliser gitignore.io (en ligne ou CLI)
# 1. Via le web : https://www.toptal.com/developers/gitignore

# 2. Via ligne de commande
# Installer gi
sudo apt install curl
echo "function gi() { curl -sL https://www.toptal.com/developers/gitignore/api/\$@ ;}" >> ~/.bashrc
source ~/.bashrc

# Générer un .gitignore
gi python,django,vscode > .gitignore
gi node,react,visualstudiocode >> .gitignore
gi java,maven,intellij >> .gitignore

# Lister les templates disponibles
gi list

# 3. Via GitHub
# Lors de la création d'un repo sur GitHub, sélectionner un template .gitignore
```

#### Bonnes Pratiques .gitignore

**✅ À FAIRE**
```bash
# Commiter le .gitignore
git add .gitignore
git commit -m "chore: add .gitignore"

# Créer un .gitignore dès le début du projet
# Utiliser des templates reconnus
# Documenter les règles complexes avec des commentaires
# Inclure un .env.example pour montrer la structure
# Tester avec git status --ignored
```

**❌ À ÉVITER**
```bash
# Ne pas ignorer les fichiers critiques :
# - README.md
# - Documentation
# - Tests
# - Fichiers de configuration d'exemple (.env.example)

# Ne pas faire :
*                         # Ignore TOUT (désastre!)
*.js                      # Trop large, ignore le code source
```

#### Template .gitignore Universel pour Démarrer

```gitignore
# Template .gitignore universel

### Secrets et Credentials ###
.env
.env.*
!.env.example
*.pem
*.key
secrets.*
credentials.*

### OS ###
.DS_Store
Thumbs.db
Desktop.ini

### IDEs ###
.vscode/
.idea/
*.swp
*.swo

### Dependencies ###
node_modules/
venv/
env/
.venv/
vendor/

### Build ###
build/
dist/
out/
target/
*.o
*.pyc
*.class

### Logs ###
logs/
*.log

### Cache ###
.cache/
tmp/
temp/
.temp/

### Backup ###
*.bak
*.backup
*.old
*.orig

### Divers ###
.DS_Store
*~
```

#### Debugging .gitignore

```bash
# Si un fichier n'est pas ignoré comme prévu :

# 1. Vérifier l'ordre des règles (les dernières prennent le dessus)
cat .gitignore

# 2. Tester explicitement
git check-ignore -v path/to/file.txt

# 3. Vérifier qu'il n'est pas déjà tracké
git ls-files | grep fichier.txt

# 4. Forcer l'ajout malgré .gitignore (derniers recours)
git add -f fichier.txt

# 5. Voir tous les .gitignore actifs
git config --get core.excludesfile  # Global
find . -name .gitignore              # Locaux
```

---

## 5. Utilisation de GitHub

### 5.1 Configuration SSH

#### Pourquoi Utiliser SSH ?

SSH (Secure Shell) offre plusieurs avantages pour GitHub :
- **Sécurité renforcée** : Authentification par clés cryptographiques
- **Pas de mot de passe** : Plus besoin de taper votre mot de passe à chaque push
- **Automatisation** : Parfait pour les scripts et CI/CD
- **Protection 2FA** : Fonctionne même avec l'authentification à deux facteurs

#### Comprendre les Clés SSH

```
Clé Privée (id_ed25519)        Clé Publique (id_ed25519.pub)
└─ Reste sur votre machine     └─ Partagée avec GitHub
└─ NE JAMAIS partager          └─ Peut être publique
└─ Comme un mot de passe       └─ Comme un cadenas
```

#### Générer une Paire de Clés SSH

```bash
# Méthode moderne (Ed25519 - Recommandée)
ssh-keygen -t ed25519 -C "votre.email@example.com"

# Pour les systèmes plus anciens (RSA)
ssh-keygen -t rsa -b 4096 -C "votre.email@example.com"

# Le processus demande :
# 1. Emplacement du fichier (appuyez sur Entrée pour le défaut)
# 2. Passphrase (optionnel mais recommandé pour la sécurité)

# Sortie attendue :
# Generating public/private ed25519 key pair.
# Enter file in which to save the key (/home/user/.ssh/id_ed25519):
# Enter passphrase (empty for no passphrase):
# Your identification has been saved in /home/user/.ssh/id_ed25519
# Your public key has been saved in /home/user/.ssh/id_ed25519.pub
```

#### Structure des Fichiers SSH

```bash
# Vérifier que les clés ont été créées
ls -la ~/.ssh/

# Vous devriez voir :
# id_ed25519           # Clé PRIVÉE (permissions 600)
# id_ed25519.pub       # Clé PUBLIQUE (permissions 644)
# known_hosts          # Hôtes connus/approuvés
# config               # Configuration SSH (optionnel)

# Vérifier les permissions (IMPORTANT pour la sécurité)
ls -l ~/.ssh/id_ed25519
# Devrait afficher : -rw------- (600)

# Si les permissions sont incorrectes :
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
chmod 700 ~/.ssh
```

#### Démarrer et Configurer l'Agent SSH

```bash
# Démarrer l'agent SSH
eval "$(ssh-agent -s)"
# Sortie : Agent pid 12345

# Ajouter votre clé privée à l'agent
ssh-add ~/.ssh/id_ed25519

# Si vous avez défini une passphrase, elle sera demandée ici

# Lister les clés chargées dans l'agent
ssh-add -l

# Supprimer une clé de l'agent
ssh-add -d ~/.ssh/id_ed25519

# Supprimer toutes les clés
ssh-add -D
```

#### Configuration Automatique de l'Agent SSH (Optionnel)

```bash
# Ajouter au fichier ~/.bashrc ou ~/.zshrc pour démarrage automatique
nano ~/.bashrc

# Ajouter ces lignes à la fin :
# Start SSH agent
if [ -z "$SSH_AUTH_SOCK" ]; then
   eval "$(ssh-agent -s)" > /dev/null
   ssh-add ~/.ssh/id_ed25519 2>/dev/null
fi

# Recharger la configuration
source ~/.bashrc
```

#### Ajouter la Clé Publique sur GitHub

```bash
# 1. Afficher et copier votre clé publique
cat ~/.ssh/id_ed25519.pub

# Ou copier directement dans le presse-papiers (Ubuntu/Linux)
cat ~/.ssh/id_ed25519.pub | xclip -selection clipboard
# Si xclip n'est pas installé :
sudo apt install xclip

# 2. Sur GitHub :
# - Aller sur https://github.com/settings/keys
# - Cliquer sur "New SSH key"
# - Titre : "Mon PC Ubuntu" (ou descriptif de votre machine)
# - Type : Authentication Key
# - Key : Coller votre clé publique
# - Cliquer sur "Add SSH key"
```

#### Tester la Connexion SSH

```bash
# Test de connexion à GitHub
ssh -T git@github.com

# Première connexion : Confirmation du fingerprint
# The authenticity of host 'github.com (...)' can't be established.
# ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
# Are you sure you want to continue connecting (yes/no/[fingerprint])? yes

# Réponse attendue :
# Hi username! You've successfully authenticated, but GitHub does not provide shell access.

# Si erreur "Permission denied" :
# 1. Vérifier que la clé est dans l'agent : ssh-add -l
# 2. Vérifier les permissions : ls -l ~/.ssh/id_ed25519
# 3. Vérifier que la clé est bien sur GitHub
```

#### Utiliser Plusieurs Clés SSH

```bash
# Créer un fichier de configuration SSH
nano ~/.ssh/config

# Exemple de configuration multi-comptes :
# Compte GitHub personnel
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes

# Compte GitHub professionnel
Host github-work
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_work
    IdentitiesOnly yes

# Utilisation :
# git clone git@github.com:username/repo.git           # Compte personnel
# git clone git@github-work:company/repo.git           # Compte pro
```

#### Sécurité des Clés SSH

```bash
# Protéger votre clé privée avec une passphrase
ssh-keygen -p -f ~/.ssh/id_ed25519

# Révoquer une clé compromise :
# 1. Sur GitHub, supprimer la clé publique
# 2. Générer une nouvelle paire de clés
# 3. Ajouter la nouvelle clé publique sur GitHub

# Sauvegarder votre clé privée (chiffrée)
# ATTENTION : Stocker dans un endroit sûr (pas dans le cloud en clair)
tar -czf ssh-backup.tar.gz ~/.ssh/id_ed25519
gpg -c ssh-backup.tar.gz  # Chiffrer avec GPG

# Audit des clés autorisées
ssh-keygen -l -f ~/.ssh/id_ed25519.pub  # Affiche le fingerprint
```

#### Conversion HTTPS vers SSH

```bash
# Vérifier l'URL actuelle du remote
git remote -v

# Changer de HTTPS vers SSH
git remote set-url origin git@github.com:username/repo.git

# Vérifier le changement
git remote -v
# origin  git@github.com:username/repo.git (fetch)
# origin  git@github.com:username/repo.git (push)
```

#### Dépannage SSH

```bash
# Mode verbeux pour debug
ssh -vT git@github.com

# Vérifier quelle clé est utilisée
ssh -vT git@github.com 2>&1 | grep "identity file"

# Tester avec une clé spécifique
ssh -i ~/.ssh/id_ed25519 -T git@github.com

# Problème de permissions
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
chmod 644 ~/.ssh/known_hosts
chmod 644 ~/.ssh/config

# Régénérer known_hosts si problème
rm ~/.ssh/known_hosts
ssh -T git@github.com  # Accepter le nouveau fingerprint
```

### 5.2 Créer et Gérer des Repositories

#### Depuis GitHub (Web)
1. Cliquer sur "New repository"
2. Nommer le repository
3. Choisir Public/Private
4. Optionnel: Initialize with README, .gitignore, license

#### Depuis Local vers GitHub
```bash
# Créer un repo local
mkdir mon-projet && cd mon-projet
git init
echo "# Mon Projet" >> README.md
git add .
git commit -m "Initial commit"

# Lier à GitHub (après création du repo sur GitHub)
git remote add origin git@github.com:username/mon-projet.git
git branch -M main
git push -u origin main
```

### 5.3 Fork et Pull Requests

#### Workflow Fork
```bash
# 1. Fork le projet sur GitHub (bouton Fork)

# 2. Clone votre fork
git clone git@github.com:votre-username/projet.git
cd projet

# 3. Ajoute le repo original comme upstream
git remote add upstream git@github.com:original-owner/projet.git

# 4. Créer une branche pour vos changements
git checkout -b feature/ma-fonctionnalite

# 5. Faire vos modifications et commits
git add .
git commit -m "Ajout de ma fonctionnalité"

# 6. Pousser vers votre fork
git push origin feature/ma-fonctionnalite

# 7. Créer une Pull Request sur GitHub
# Aller sur GitHub et cliquer "Compare & pull request"

# 8. Garder votre fork à jour
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

### 5.4 Issues et Projects

#### Gestion des Issues
```bash
# Sur GitHub:
# - Issues > New issue
# - Utiliser des labels (bug, enhancement, documentation)
# - Assigner des personnes
# - Lier à des milestones
# - Référencer dans commits: "Fix #42"

# Templates d'issues:
# Créer .github/ISSUE_TEMPLATE/bug_report.md
# Créer .github/ISSUE_TEMPLATE/feature_request.md
```

#### GitHub Projects
- Créer un project board (Kanban)
- Colonnes: To Do, In Progress, Review, Done
- Lier issues et PRs
- Automatiser les déplacements

### 5.5 Branches et Branch Protection

#### Stratégies de Branches

**Git Flow:**
```
main (production)
├── develop (développement)
    ├── feature/nouvelle-fonction
    ├── feature/autre-fonction
    └── release/v1.2.0
hotfix/correction-urgente (depuis main)
```

**GitHub Flow (simplifié):**
```
main (toujours déployable)
├── feature/ma-fonctionnalite
├── fix/correction-bug
└── docs/mise-a-jour-readme
```

#### Branch Protection (GitHub Settings)
```bash
# Sur GitHub: Settings > Branches > Add rule
# - Require pull request reviews
# - Require status checks to pass
# - Require branches to be up to date
# - Include administrators
# - Restrict who can push
```

### 5.6 GitHub Actions (CI/CD)

#### Exemple de Workflow
```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.11'
    
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
    
    - name: Run tests
      run: |
        pytest tests/
    
    - name: Run security checks
      run: |
        pip install bandit
        bandit -r src/
```

### 5.7 Releases et Versioning

```bash
# Créer une release sur GitHub:
# 1. Créer et pousser un tag
git tag -a v1.0.0 -m "Version 1.0.0 - Première version stable"
git push origin v1.0.0

# 2. Sur GitHub: Releases > Draft a new release
# 3. Sélectionner le tag
# 4. Rédiger les release notes
# 5. Ajouter des binaires/assets si nécessaire
# 6. Publier

# Semantic Versioning (MAJOR.MINOR.PATCH):
# MAJOR: changements incompatibles
# MINOR: nouvelles fonctionnalités compatibles
# PATCH: corrections de bugs
```

### 5.8 Collaboration en Équipe

#### Organisation sur GitHub
```bash
# Créer une organisation
# - Settings > Organizations > New organization
# - Inviter des membres
# - Créer des équipes
# - Gérer les permissions par équipe

# Permissions:
# - Read: clone et fork
# - Triage: gérer issues et PRs
# - Write: push
# - Maintain: gérer repo sans actions destructives
# - Admin: contrôle total
```

#### Code Review Best Practices
```bash
# Lors d'une Pull Request:
# 1. Description claire des changements
# 2. Lier les issues concernées
# 3. Tests passent
# 4. Code coverage maintenu
# 5. Documentation mise à jour

# Review checklist:
# - Le code respecte les standards
# - Tests adéquats
# - Pas de secrets/credentials
# - Performance acceptable
# - Sécurité vérifiée
```

### 5.9 GitHub CLI

```bash
# Installation
sudo apt install gh

# Authentification
gh auth login

# Gérer les repos
gh repo create mon-projet --public
gh repo clone owner/repo
gh repo view
gh repo fork owner/repo

# Gérer les PRs
gh pr create --title "Mon PR" --body "Description"
gh pr list
gh pr view 42
gh pr checkout 42
gh pr merge 42

# Gérer les issues
gh issue create --title "Bug" --body "Description"
gh issue list
gh issue view 42
gh issue close 42

# Workflows
gh workflow list
gh workflow view
gh run list
gh run watch
```

### 5.10 Secrets et Security

```bash
# Ne JAMAIS commiter:
# - Mots de passe
# - Clés API
# - Tokens
# - Certificats privés
# - Fichiers de configuration sensibles

# Utiliser GitHub Secrets:
# Settings > Secrets and variables > Actions > New repository secret

# Dans GitHub Actions:
# ${{ secrets.API_KEY }}

# Scanner les secrets accidentels:
git secrets --install
git secrets --register-aws
git secrets --scan

# GitHub Security Features:
# - Dependabot alerts (vulnérabilités dépendances)
# - Code scanning (analyse statique)
# - Secret scanning (détection de secrets)
# - Security advisories
```

### 5.11 Licences Logicielles (Licenses)

#### Pourquoi une Licence ?

Une licence définit les **droits et obligations** concernant l'utilisation, la modification et la distribution de votre code :
- **Protection légale** : Clarifie ce que les autres peuvent faire
- **Contribution** : Encourage ou restreint les contributions
- **Usage commercial** : Autorise ou interdit l'usage commercial
- **Responsabilité** : Limite votre responsabilité légale

**⚠️ IMPORTANT** : Sans licence, votre code est sous **copyright par défaut** (tous droits réservés), ce qui signifie que personne n'a le droit de l'utiliser, le modifier ou le redistribuer.

#### Types de Licences

##### Licences Permissives (Libérales)

**MIT License** 🌟 La plus populaire
```
✅ Utilisation commerciale
✅ Modification
✅ Distribution
✅ Utilisation privée
❌ Responsabilité limitée
❌ Pas de garantie
```
- **Idéal pour** : Projets open source maximisant l'adoption
- **Utilisé par** : jQuery, Rails, Node.js, React
- **En résumé** : "Faites ce que vous voulez, mais je ne suis pas responsable"

**Apache License 2.0**
```
✅ Utilisation commerciale
✅ Modification
✅ Distribution
✅ Brevet explicite
✅ Protection de marque
❌ Responsabilité limitée
```
- **Idéal pour** : Projets avec brevets logiciels
- **Utilisé par** : Android, Apache, Swift, Kubernetes
- **En résumé** : Comme MIT + protection brevets

**BSD License (2-Clause et 3-Clause)**
```
✅ Utilisation commerciale
✅ Modification
✅ Distribution
❌ Responsabilité limitée
```
- **Idéal pour** : Compatibilité maximale
- **Utilisé par** : FreeBSD, Django
- **En résumé** : Très similaire à MIT

##### Licences Copyleft (Protectrices)

**GNU GPL v3 (General Public License)**
```
✅ Utilisation commerciale
✅ Modification
✅ Distribution
⚠️ Obligation de partager le code modifié
⚠️ Même licence pour les dérivés
❌ Responsabilité limitée
```
- **Idéal pour** : Garantir que le code reste libre
- **Utilisé par** : Linux, Git, WordPress
- **En résumé** : "Si vous modifiez, vous devez partager avec la même licence"

**GNU AGPL v3 (Affero GPL)**
```
✅ Tout comme GPL v3
⚠️ + Obligation même pour usage réseau/SaaS
```
- **Idéal pour** : Services web et SaaS
- **Utilisé par** : MongoDB (avant), Mastodon
- **En résumé** : Comme GPL mais couvre aussi l'utilisation via réseau

**LGPL v3 (Lesser GPL)**
```
✅ Utilisation dans logiciels propriétaires (comme bibliothèque)
⚠️ Modifications de la bibliothèque doivent être partagées
```
- **Idéal pour** : Bibliothèques
- **Utilisé par** : Qt, GTK
- **En résumé** : GPL plus souple pour les bibliothèques

##### Licences Spéciales

**Creative Commons (CC)**
- **CC0** : Domaine public, aucun droit réservé
- **CC-BY** : Attribution requise
- **CC-BY-SA** : Attribution + ShareAlike (copyleft)
- **Idéal pour** : Documentation, contenu créatif
- **PAS pour** : Code logiciel (utiliser MIT/GPL à la place)

**Unlicense**
```
✅ Domaine public
✅ Aucune restriction
```
- **Idéal pour** : Dédier au domaine public
- **En résumé** : "Prenez-le, il est à vous"

**WTFPL (Do What The Fuck You Want To Public License)**
```
✅ Faites absolument ce que vous voulez
```
- **Idéal pour** : Projets humoristiques/expérimentaux
- **Non recommandé pour** : Projets professionnels

#### Choisir une Licence

##### Guide de Décision Rapide

```
Vous voulez que votre code :

1. Soit utilisé le plus largement possible ?
   └─→ MIT License

2. Protège vos brevets ?
   └─→ Apache License 2.0

3. Reste toujours libre et open source ?
   └─→ GPL v3

4. Soit utilisé dans des applications propriétaires ?
   ├─→ MIT ou Apache (pour bibliothèques)
   └─→ LGPL (si vous voulez quand même du copyleft)

5. Soit un service web qui reste libre ?
   └─→ AGPL v3

6. Soit du domaine public ?
   └─→ Unlicense ou CC0
```

##### Comparaison Visuelle

```
Permissivité (du plus au moins permissif) :

Unlicense
    ↓
MIT / BSD
    ↓
Apache 2.0
    ↓
LGPL
    ↓
GPL
    ↓
AGPL

Copyright complet (tous droits réservés)
```

#### Ajouter une Licence à votre Projet

##### Méthode 1 : Via GitHub (Recommandé)

```bash
# Lors de la création du repo sur GitHub :
# 1. Cocher "Add a README file"
# 2. Sélectionner "Add a license" → Choisir la licence
# 3. Create repository

# Pour un repo existant :
# 1. Sur GitHub : Add file > Create new file
# 2. Nom du fichier : LICENSE ou LICENSE.txt
# 3. GitHub détecte et propose des templates
# 4. Choisir votre licence
# 5. Commit
```

##### Méthode 2 : Manuellement

```bash
# 1. Créer le fichier LICENSE à la racine
nano LICENSE

# 2. Copier le texte de la licence depuis :
# - https://choosealicense.com/
# - https://opensource.org/licenses/
# - https://www.gnu.org/licenses/

# 3. Remplacer [year] et [fullname] par vos informations

# 4. Commiter
git add LICENSE
git commit -m "docs: add MIT License"
git push
```

##### Méthode 3 : Avec GitHub CLI

```bash
# Ajouter une licence avec gh CLI
gh repo edit --license mit

# Lister les licences disponibles
gh repo license list
```

#### Template de Licence MIT

```text
MIT License

Copyright (c) 2025 Votre Nom

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

#### Template de Licence Apache 2.0

```text
                                 Apache License
                           Version 2.0, January 2004
                        http://www.apache.org/licenses/

   Copyright 2025 Votre Nom

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
```

#### Headers de Licence dans les Fichiers

##### Pour MIT/Apache
```python
# Python example
"""
Copyright (c) 2025 Votre Nom

This file is part of Nom-du-Projet.

Licensed under the MIT License. See LICENSE file in the project root.
"""
```

```javascript
// JavaScript example
/**
 * Copyright (c) 2025 Votre Nom
 * 
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * See LICENSE file in the project root for details.
 */
```

##### Pour GPL
```python
# Python example
"""
Copyright (C) 2025 Votre Nom

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program. If not, see <https://www.gnu.org/licenses/>.
"""
```

#### Badges de Licence pour README

```markdown
<!-- MIT -->
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

<!-- Apache 2.0 -->
[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

<!-- GPL v3 -->
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

<!-- AGPL v3 -->
[![License: AGPL v3](https://img.shields.io/badge/License-AGPL_v3-blue.svg)](https://www.gnu.org/licenses/agpl-3.0)

<!-- BSD 3-Clause -->
[![License](https://img.shields.io/badge/License-BSD_3--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)

<!-- Unlicense -->
[![License: Unlicense](https://img.shields.io/badge/license-Unlicense-blue.svg)](http://unlicense.org/)
```

#### Licences Multiples (Dual Licensing)

```bash
# Certains projets offrent plusieurs licences
# Exemple : GPL pour usage libre, licence commerciale pour usage propriétaire

# Dans LICENSE :
```
This project is dual-licensed under:
1. GNU GPL v3 - for open source use
2. Commercial License - for proprietary use

See LICENSE-GPL and LICENSE-COMMERCIAL for details.
```

# Créer deux fichiers :
LICENSE-GPL
LICENSE-COMMERCIAL
```

#### Compatibilité des Licences

```
Projets MIT peuvent inclure :
✅ MIT, Apache, BSD, Unlicense
❌ GPL (incompatible dans certains cas)

Projets Apache peuvent inclure :
✅ Apache, MIT, BSD, Unlicense
❌ GPL v2 (incompatible)
⚠️ GPL v3 (compatible mais devient GPL)

Projets GPL v3 peuvent inclure :
✅ GPL v3, LGPL, Apache 2.0, MIT, BSD
❌ GPL v2 (sans compatibilité explicite)
❌ Licences propriétaires

Projets propriétaires peuvent inclure :
✅ MIT, Apache, BSD, Unlicense
❌ GPL, AGPL (nécessitent ouverture du code)
⚠️ LGPL (sous conditions)
```

#### Vérifier les Licences des Dépendances

```bash
# Python
pip install pip-licenses
pip-licenses

# Node.js
npm install -g license-checker
license-checker

# Vérifier les incompatibilités
npm install -g legally
legally

# GitHub peut scanner automatiquement
# Settings > Security > Dependency graph > Insights
```

#### Changer de Licence

```bash
# Attention : Nécessite accord de TOUS les contributeurs

# 1. Vérifier les contributeurs
git log --format='%aN' | sort -u

# 2. Contacter tous les contributeurs pour accord
# 3. Créer une issue pour transparence
# 4. Une fois accord obtenu :
git rm LICENSE
# Ajouter nouvelle licence
git add LICENSE
git commit -m "docs: change license from MIT to Apache 2.0"

# 5. Mettre à jour README et headers de fichiers
```

#### FAQ Licences

**Q: Dois-je mettre une licence sur mon projet personnel ?**  
R: Oui ! Sans licence, personne ne peut légalement utiliser votre code.

**Q: Puis-je changer de licence plus tard ?**  
R: Oui, mais compliqué si vous avez des contributeurs (besoin de leur accord).

**Q: Quelle licence pour un projet universitaire ?**  
R: MIT est généralement un bon choix (simple et permissif).

**Q: GPL m'oblige-t-il à publier mon code ?**  
R: Seulement si vous distribuez le logiciel. Usage interne = pas d'obligation.

**Q: Puis-je utiliser du code MIT dans mon projet commercial ?**  
R: Oui, totalement ! C'est le but de MIT.

**Q: Dois-je mettre la licence dans chaque fichier ?**  
R: Pas obligatoire mais recommandé pour les gros projets.

**Q: Licence pour un projet privé/interne ?**  
R: Pas nécessaire si vraiment privé, mais prévoir si publication future.

#### Ressources

- **Choisir une licence** : https://choosealicense.com/
- **Comparaison** : https://www.gnu.org/licenses/license-list.html
- **SPDX (identifiants standard)** : https://spdx.org/licenses/
- **TL;DR Legal** : https://www.tldrlegal.com/ (explications simples)
- **License Compatibility** : https://dwheeler.com/essays/floss-license-slide.html

#### Checklist Licence

Avant de publier un projet :

- [ ] Fichier LICENSE présent à la racine
- [ ] Année et nom corrects dans la licence
- [ ] Badge de licence dans le README
- [ ] Section "License" dans le README
- [ ] Headers de licence dans fichiers (si applicable)
- [ ] Licence compatible avec les dépendances
- [ ] Licence cohérente avec objectifs du projet
- [ ] Tous les contributeurs informés (si changement)



---

## 6. Bonnes Pratiques pour Applications Sécurisées

### 6.1 Principes de Sécurité

#### Principe du Moindre Privilège
```bash
# Créer des utilisateurs dédiés pour les applications
sudo adduser --system --group appuser
sudo chown -R appuser:appuser /opt/mon-app

# Ne jamais exécuter d'applications en tant que root
# Utiliser sudo uniquement quand nécessaire
```

#### Gestion des Secrets
```bash
# Utiliser des variables d'environnement
export DATABASE_PASSWORD="secret"

# Fichiers de configuration hors du repository
echo ".env" >> .gitignore
echo "config.local.yml" >> .gitignore

# Utiliser des outils de gestion de secrets
# - HashiCorp Vault
# - AWS Secrets Manager
# - Azure Key Vault
# - Git-crypt pour chiffrer dans git
```

### 6.2 Sécurité du Code

#### Validation des Entrées
```python
# TOUJOURS valider et sanitizer les entrées utilisateur
# JAMAIS faire confiance aux données externes
# Utiliser des bibliothèques de validation
```

#### Chiffrement
```bash
# Utiliser HTTPS (TLS/SSL)
# Chiffrer les données sensibles au repos
# Utiliser des algorithmes modernes (AES-256, RSA-2048+)

# Générer des certificats Let's Encrypt
sudo apt install certbot
sudo certbot --nginx -d example.com
```

#### Mises à Jour
```bash
# Maintenir le système à jour
sudo apt update && sudo apt upgrade -y

# Activer les mises à jour automatiques de sécurité
sudo apt install unattended-upgrades
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

### 6.3 Audit et Logs

```bash
# Configurer la rotation des logs
sudo nano /etc/logrotate.d/mon-app

# Monitorer les logs système
sudo tail -f /var/log/syslog
sudo tail -f /var/log/auth.log

# Utiliser fail2ban pour bannir les IPs suspectes
sudo apt install fail2ban
sudo systemctl enable fail2ban
```

### 6.4 Checklist de Sécurité Projet

- [ ] Tous les secrets sont hors du code source
- [ ] .gitignore configuré correctement
- [ ] SSH configuré avec clés (pas de mots de passe)
- [ ] Pare-feu (UFW) configuré
- [ ] Services non nécessaires désactivés
- [ ] Utilisateurs avec privilèges minimaux
- [ ] Mises à jour automatiques activées
- [ ] HTTPS/TLS configuré
- [ ] Validation des entrées implémentée
- [ ] Logs configurés et monitorés
- [ ] Backups réguliers
- [ ] Tests de sécurité (SAST/DAST)
- [ ] Dépendances à jour (Dependabot)
- [ ] Code review obligatoire
- [ ] CI/CD avec tests de sécurité

### 6.5 Ressources et Outils

#### Outils de Sécurité
```bash
# Analyse statique de code
pip install bandit                     # Python
npm install -g eslint-plugin-security  # JavaScript

# Scan de vulnérabilités
npm audit                              # Node.js
pip-audit                              # Python
trivy image mon-image                  # Docker

# Scan de secrets
pip install detect-secrets
detect-secrets scan > .secrets.baseline

# Analyse de dépendances
snyk test
```

#### Ressources
- OWASP Top 10
- CWE (Common Weakness Enumeration)
- CVE (Common Vulnerabilities and Exposures)
- NIST Cybersecurity Framework
- Documentation Ubuntu Security

---

## Conclusion

Ce guide couvre les fondamentaux essentiels pour le développement d'applications sécurisées sur Ubuntu avec Git et GitHub. La sécurité doit être intégrée dès le début du projet et maintenue tout au long du cycle de développement.

### Points Clés à Retenir

1. **Ubuntu/Linux**: Maîtriser la ligne de commande et l'administration système
2. **Bash**: Automatiser les tâches répétitives
3. **Git**: Versionner tout le code, commiter souvent, messages clairs
4. **GitHub**: Collaborer efficacement, utiliser PRs et code reviews
5. **Sécurité**: Principe du moindre privilège, ne jamais commiter de secrets, maintenir à jour

### Prochaines Étapes

1. Pratiquer quotidiennement les commandes
2. Contribuer à des projets open source
3. Mettre en place un projet avec CI/CD
4. Suivre les actualités de sécurité
5. Approfondir avec des certifications (Linux+, Security+)

---

**Document créé pour le suivi du projet**  
*Conception et Développement d'Applications Sécurisées*  
Date: Novembre 2025