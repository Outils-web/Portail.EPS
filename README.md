# 🏃 Portail EPS — M. Moraux

Un mini-site web moderne servant de **portail centralisé** pour tous les sites et ressources liés à l'**Éducation Physique et Sportive**.

---

## 🖼️ Aperçu

- Design sportif et dynamique avec thème sombre/clair
- Cartes interactives organisées par catégories
- Barre de recherche en temps réel
- Filtres par catégorie (Athlétisme, Fitness, Sports Collectifs…)
- Possibilité d'ajouter des sites via un accès protégé par mot de passe
- Sauvegarde automatique dans le navigateur (localStorage)
- 100% responsive (mobile & desktop)

---

## 🚀 Utilisation

### Lancement

Il suffit d'ouvrir le fichier `index.html` dans n'importe quel navigateur moderne.  
Aucune installation, aucun serveur, aucune dépendance à installer.

### Naviguer

- Utilisez la **barre de recherche** pour filtrer les sites par nom, description ou catégorie.
- Cliquez sur un **filtre catégorie** (pastilles colorées) pour n'afficher qu'une catégorie.
- Cliquez sur le bouton **"Ouvrir le site"** d'une carte pour accéder à la ressource dans un nouvel onglet.

### Thème sombre / clair

Cliquez sur l'icône 🌙 / ☀️ en haut à droite pour basculer entre les deux thèmes.  
Votre préférence est mémorisée automatiquement.

---

## ➕ Ajouter un nouveau site

1. Cliquez sur le bouton **"Ajouter un site"** en haut à droite.
2. Entrez le mot de passe d'accès (`eps` par défaut).
3. Remplissez le formulaire :
   - **Nom du site** : titre court et descriptif
   - **Description** : résumé en une ou deux phrases
   - **URL** : adresse complète (commençant par `https://`)
   - **Catégorie** : choisir parmi les catégories existantes ou "Autre"
4. Cliquez sur **"Ajouter au portail"**.

La carte apparaît immédiatement et est **sauvegardée dans votre navigateur** (localStorage).  
Elle persistera même après fermeture et réouverture du site.

### Ajouter un site directement dans le code

Pour intégrer un site de façon permanente (sans passer par l'interface), modifiez le tableau `INITIAL_SITES` dans la balise `<script>` du fichier `index.html` :

```javascript
const INITIAL_SITES = [
  // ... sites existants ...
  {
    id: "mon-nouveau-site",           // identifiant unique (sans espaces)
    title: "Nom de mon site",
    desc: "Description courte du site.",
    url: "https://exemple.com",
    cat: "Athlétisme",                // catégorie existante ou nouvelle
    user: false
  }
];
```

Pour ajouter une **nouvelle catégorie** avec ses propres couleurs et icône, ajoutez une entrée dans l'objet `CAT_STYLES` :

```javascript
const CAT_STYLES = {
  // ... catégories existantes ...
  "Ma Catégorie": {
    accent: "#facc15",               // couleur principale (hex)
    iconBg: "rgba(250,204,21,.15)",  // même couleur avec opacité
    icon: "fa-trophy"                // icône Font Awesome (sans le préfixe "fa-solid")
  }
};
```

---

## 🔐 Sécurisation du mot de passe

> ⚠️ **Avertissement important**  
> La protection par mot de passe de ce portail est une **sécurité côté client uniquement**.  
> Elle est conçue pour **éviter les modifications accidentelles**, pas pour protéger des données sensibles.  
> N'importe qui qui inspecte le code source du navigateur peut potentiellement la contourner.

### Comment ça fonctionne ?

1. Le mot de passe n'est **jamais stocké en clair** dans le code.
2. Lorsque vous cliquez sur "Ajouter un site", le mot de passe saisi est **haché en SHA-256** via l'API Web Crypto du navigateur.
3. Ce hash est comparé à une **valeur de référence pré-calculée** (le hash SHA-256 du mot de passe `eps`).
4. Le code est partiellement **obfusqué** : les octets du mot de passe sont stockés inversés pour ne pas apparaître en clair dans le source.

### Changer le mot de passe

Pour utiliser un autre mot de passe, procédez ainsi :

1. Ouvrez la console de votre navigateur (F12 → Console).
2. Tapez la commande suivante en remplaçant `nouveau_mdp` :
   ```javascript
   const buf = await crypto.subtle.digest('SHA-256', new TextEncoder().encode('nouveau_mdp'));
   console.log([...new Uint8Array(buf)].map(b=>b.toString(16).padStart(2,'0')).join(''));
   ```
3. Copiez le hash affiché.
4. Dans `index.html`, remplacez la ligne `_raw` par les codes ASCII de votre nouveau mot de passe inversé, ou adaptez la logique de décodage.

---

## 🛠️ Technologies utilisées

| Technologie | Usage |
|---|---|
| **HTML5** | Structure de la page |
| **CSS3** | Design, animations, flexbox/grid, variables CSS |
| **JavaScript (Vanilla ES2022)** | Interactivité, filtres, modal, localStorage |
| **Web Crypto API** | Hachage SHA-256 du mot de passe |
| **Font Awesome 6** (CDN) | Icônes |
| **Google Fonts** (CDN) | Typographies : Bebas Neue + Nunito |
| **localStorage** | Sauvegarde des sites ajoutés par l'utilisateur |

Aucun framework, aucune dépendance npm, aucun build nécessaire.

---

## 📁 Structure du projet

```
portail-eps/
├── index.html   ← Site complet (HTML + CSS + JS intégrés)
└── README.md    ← Cette documentation
```

---

## 📜 Licence

Projet pédagogique — M. Moraux. Usage libre dans un contexte éducatif.
