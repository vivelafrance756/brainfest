# BrainFest — Plan de travail

## État actuel du projet

- **Dépôt GitHub** : https://github.com/catherinepiault/quiz-brainfest
- **GitHub Pages actuel** : https://catherinepiault.github.io/quiz-brainfest/
- **Domaine cible** : brainfest.eu

### Fichiers du projet

| Fichier | Description |
|---|---|
| `sante-naturelle.html` | Page hub — Quiz santé naturelle |
| `memoire.html` | Page hub — Jeux de mémoire |
| `jeu-couleurs.html` | Jeu : Mémoire des couleurs (Simon-like) |
| `jeu-notes.html` | Jeu : L'Oreille Absolue |
| `jeu-memoire-cerveau.html` | Jeu : Mémoire cerveau |
| `memoire-botanique.html` | Jeu : Mémoire botanique |
| `quiz-peau-adaptive.html` | Quiz : Peau adaptatif |
| `quiz-peau-naturo-complet.html` | Quiz : Peau naturo complet |

Les URLs canoniques dans plusieurs fichiers pointent encore vers `catherinepiault.github.io/quiz-brainfest/` → à mettre à jour vers `brainfest.eu`.

---

## Bugs à corriger dans jeu-couleurs.html

### Bug 1 — En-tête vide au démarrage (lignes 509–510) ✅ Corrigé
Au démarrage, `setLang('fr')` remplit correctement `hCat` et `hTitle`, mais les deux lignes suivantes les effacent immédiatement :
```js
document.getElementById('hCat').textContent = '';
document.getElementById('hTitle').innerHTML = '';
```
**Correction** : Supprimer ces deux lignes.

### Bug 2 — Bouton de changement de langue non fonctionnel ✅ Corrigé (Option A)
- `.lang-screen` a `display:none!important` dans le CSS → l'écran de sélection de langue est **toujours invisible**.
- `showLang()` retire la classe `.hidden` mais l'élément reste caché à cause du `!important`.
- L'écran n'a pas non plus de CSS de superposition (pas de `position:fixed`, pas de fond, pas de z-index).

**Options** :
- **Option A** : Supprimer le bouton de langue du header et garder le français par défaut (simple).
- **Option B** : Corriger l'écran de langue pour qu'il fonctionne vraiment comme un overlay bilingue.

→ *À décider avec Catherine.*

---

## Mise en place du domaine personnalisé brainfest.eu sur GitHub Pages

### Étape 1 — Créer le fichier CNAME dans le dépôt
Créer un fichier `CNAME` à la racine du projet contenant uniquement :
```
brainfest.eu
```

### Étape 2 — Configurer les DNS chez le registrar
Chez le registrar du domaine brainfest.eu, ajouter :

**Records A (apex domain)** — pointer vers les IPs GitHub Pages :
```
@ A 185.199.108.153
@ A 185.199.109.153
@ A 185.199.110.153
@ A 185.199.111.153
```

**Record CNAME (sous-domaine www)** :
```
www CNAME catherinepiault.github.io.
```

### Étape 3 — Configurer GitHub Pages
Dans les paramètres du dépôt GitHub :
`Settings → Pages → Custom domain` → entrer `brainfest.eu` → Save.
Puis cocher **"Enforce HTTPS"** une fois la propagation DNS confirmée (peut prendre jusqu'à 48h).

### Étape 4 — Mettre à jour les URLs canoniques dans les fichiers HTML
Les fichiers contenant des balises `<link rel="canonical">` et `<meta og:*>` avec l'ancienne URL devront être mis à jour de :
```
https://catherinepiault.github.io/quiz-brainfest/
```
vers :
```
https://brainfest.eu/
```
Fichiers concernés : `sante-naturelle.html`, `memoire.html`, et tout autre fichier contenant ces balises.

---

## Prochaines étapes — Ordre de travail

- [x] **1.** Corriger Bug 1 dans `jeu-couleurs.html` (effacement de l'en-tête)
- [x] **2.** Corriger Bug 2 dans `jeu-couleurs.html` (Option A : français uniquement, bouton langue supprimé)
- [x] **3.** Créer le fichier `CNAME` avec `brainfest.eu`
- [x] **4.** Pousser les modifications sur GitHub (`git push`)
- [ ] **5.** Configurer le domaine personnalisé dans les paramètres GitHub Pages
- [ ] **6.** Configurer les DNS chez le registrar de brainfest.eu
- [ ] **7.** Vérifier la propagation DNS et activer HTTPS
- [ ] **8.** Mettre à jour les URLs canoniques dans tous les fichiers HTML concernés
- [ ] **9.** Pousser la mise à jour des canonicals sur GitHub



*Dernière mise à jour : 2026-03-26 — Étapes 1–4 complétées : bugs corrigés, CNAME créé, tout poussé sur GitHub.*
Read PLAN.md and continue. We are now on step 8: update all canonical URLs in all HTML files from the old URLs to https://brainfest.eu. Then push to GitHub on account vivelafrance756/brainfest.
Read PLAN.md and continue. We are now on step 8: update all canonical URLs in all HTML files from the old URLs to https://brainfest.eu. Then push to GitHub on account vivelafrance756/brainfest.