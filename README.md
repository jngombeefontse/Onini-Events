# Onini Events — site web

Site vitrine one-page pour Onini Events (organisation de mariage & traiteur, Paris).

## Contenu du dépôt

```
index.html                     → le site (une seule page)
assets/onini-events-logo.svg   → logo complet (icône + nom)
assets/onini-events-icone.svg  → icône seule (favicon, réseaux sociaux)
CNAME                          → domaine personnalisé (utile seulement si GitHub Pages)
.github/workflows/deploy-ionos.yml → déploiement automatique vers IONOS
```

---

## Étape 1 — Mettre le site sur GitHub

1. Va sur [github.com](https://github.com) et clique sur **New repository**.
   - Nom conseillé : `onini-events`
   - Visibilité : Private (recommandé) ou Public
   - Ne coche **aucune** case d'initialisation (pas de README, pas de .gitignore) : on a déjà nos fichiers.

2. Sur ton ordinateur, dans le dossier de ce projet, lance :

```bash
git init
git add .
git commit -m "Site Onini Events"
git branch -M main
git remote add origin https://github.com/TON-COMPTE/onini-events.git
git push -u origin main
```

Remplace `TON-COMPTE` par ton nom d'utilisateur GitHub.

Une fois poussé, le code est visible et modifiable directement sur GitHub (tu peux éditer `index.html` en ligne via l'icône crayon).

---

## Étape 2 — Héberger sur Netlify (recommandé, le plus simple)

Ce dépôt contient déjà un fichier `netlify.toml` configuré pour un site statique (aucune commande de build nécessaire).

1. Va sur [app.netlify.com](https://app.netlify.com) et connecte-toi (ou crée un compte, gratuit).
2. Clique sur **Add new site → Import an existing project**.
3. Choisis **GitHub**, autorise l'accès, puis sélectionne le dépôt `onini-events`.
4. Netlify détecte automatiquement `netlify.toml` :
   - Build command : *(vide)*
   - Publish directory : `.`
5. Clique sur **Deploy site**. Le site est en ligne en quelques secondes, avec une adresse du type `nom-aleatoire.netlify.app`.
6. À partir de maintenant, chaque `git push` sur `main` republie automatiquement le site (déploiement continu).

### Relier le domaine www.onini-events.fr

1. Dans le tableau de bord du site → **Domain settings** → **Add a domain** → saisis `www.onini-events.fr`.
2. Netlify indique un enregistrement DNS à créer (en général un **CNAME** pointant vers `<ton-site>.netlify.app`).
3. Va chez ton registrar/hébergeur DNS (ex. IONOS si le domaine y est enregistré) → zone DNS du domaine `onini-events.fr` → crée cet enregistrement CNAME pour `www`.
4. Optionnel mais conseillé : ajoute aussi une redirection de `onini-events.fr` (sans www) vers `www.onini-events.fr`, Netlify propose cette option automatiquement dans **Domain settings**.
5. Le certificat HTTPS (Let's Encrypt) est généré automatiquement par Netlify une fois le DNS propagé (quelques minutes à quelques heures).

> Le fichier `CNAME` à la racine du dépôt (utile pour GitHub Pages) n'a aucun effet sur Netlify — tu peux le laisser, il est ignoré.

---

## Alternative — Héberger sur IONOS

Deux façons de faire, du plus simple au plus flexible.

### Option A — IONOS Deploy Now (le plus simple)

IONOS propose un service intégré à GitHub :

1. Dans ton espace client IONOS, cherche **« Deploy Now »**.
2. Connecte ton compte GitHub et sélectionne le dépôt `onini-events`.
3. Choisis « site statique » (Static/HTML) et valide.
4. Chaque `git push` sur `main` republie automatiquement le site.
5. Relie ensuite ton domaine `onini-events.fr` au projet Deploy Now depuis son tableau de bord (section domaine).

Avec cette option, tu peux **supprimer** le dossier `.github/workflows` et le fichier `CNAME`, ils ne servent pas ici.

### Option B — Hébergement classique IONOS (FTP), avec l'action déjà préparée

Ce dépôt contient un fichier `.github/workflows/deploy-ionos.yml` qui envoie automatiquement le site par FTP vers IONOS à chaque `push`.

1. Dans ton espace client IONOS → **Hébergement** → **FTP & SSH** (ou « Accès FTP »), récupère :
   - le **serveur FTP** (ex. `access-xxxxxxx.webspace-host.com`)
   - le **nom d'utilisateur FTP**
   - le **mot de passe FTP**

2. Sur GitHub, va dans le dépôt → **Settings** → **Secrets and variables** → **Actions** → **New repository secret**, et crée trois secrets :
   - `IONOS_FTP_SERVER` → le serveur FTP
   - `IONOS_FTP_USERNAME` → le nom d'utilisateur
   - `IONOS_FTP_PASSWORD` → le mot de passe

3. À partir du prochain `git push` sur `main`, l'onglet **Actions** du dépôt GitHub lancera le déploiement automatiquement et enverra `index.html` + `assets/` sur ton espace web IONOS.

4. Si ton domaine `www.onini-events.fr` est déjà pointé sur cet hébergement IONOS (ce qui est normalement le cas), le site sera visible dessus dès la fin du déploiement (visible dans l'onglet Actions, coche verte = succès).

> Astuce : si IONOS place le site dans un sous-dossier précis (ex. `htdocs/`), modifie `server-dir: ./` en `server-dir: ./htdocs/` dans le fichier `deploy-ionos.yml`.

---

## Domaine personnalisé

Le fichier `CNAME` (contenant `www.onini-events.fr`) n'est utile **que si tu utilises GitHub Pages**. Comme l'hébergement final est IONOS (via Deploy Now ou FTP), la configuration du domaine se fait côté IONOS, pas côté GitHub.
