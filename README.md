# Portfolio — fatihturhan.dev

CV en ligne de Fatih Turhan : une seule page statique (`index.html`, CSS écrit à la main, police
Archivo depuis Google Fonts), sans JavaScript ni étape de build.

Sections : projets en cours, expérience, compétences, formation, langues, contact.

## Voir le site en local

```bash
python3 -m http.server 8765
```

Puis ouvrir http://localhost:8765.

## Déploiement

Le site est publié par **GitHub Pages** : chaque push sur `main` lance le workflow
`.github/workflows/pages.yml`, qui publie `index.html`, le CV en PDF et le logo.
Adresse par défaut : https://fturhan.github.io/portfolio/

(`.gitlab-ci.yml` sert à l'ancien déploiement GitLab Pages, sur le remote `gitlab`.)

### Brancher le domaine fatihturhan.dev (une fois)

1. **OVH** (Web Cloud → Noms de domaine → fatihturhan.dev) :
   - Onglet **Redirection** : supprimer la redirection de `fatihturhan.dev` vers
     `www.fatihturhan.dev`. Elle crée l'enregistrement `A 213.186.33.5` et le TXT
     `"1|www.fatihturhan.dev"`, qui doivent disparaître.
   - Onglet **Zone DNS** :

     | Sous-domaine | Type | Cible |
     |---|---|---|
     | *(vide)* | A | `185.199.108.153` |
     | *(vide)* | A | `185.199.109.153` |
     | *(vide)* | A | `185.199.110.153` |
     | *(vide)* | A | `185.199.111.153` |
     | *(vide)* | AAAA | `2606:50c0:8000::153` |
     | *(vide)* | AAAA | `2606:50c0:8001::153` |
     | *(vide)* | AAAA | `2606:50c0:8002::153` |
     | *(vide)* | AAAA | `2606:50c0:8003::153` |
     | `www` | CNAME | `fturhan.github.io.` |

     Supprimer l'ancien `A` de `www` (`57.128.25.21`). Ne pas toucher aux enregistrements MX
     et SPF (messagerie OVH) ni à `ecoroulez`.
2. **GitHub**, une fois le DNS propagé : **Settings → Pages → Custom domain** :
   `fatihturhan.dev`, puis cocher **Enforce HTTPS** quand le certificat est prêt. Ou en
   ligne de commande :

   ```bash
   gh api -X PUT repos/fturhan/portfolio/pages -f cname=fatihturhan.dev
   ```

3. Recommandé : **Settings (du compte) → Pages → Add a domain** pour vérifier
   `fatihturhan.dev` (un TXT `_github-pages-challenge-fturhan` à ajouter chez OVH), ce qui
   empêche un autre compte GitHub de s'en servir.

Le domaine `.dev` impose le HTTPS dans tous les navigateurs : le site n'est joignable qu'une
fois le certificat émis par GitHub.

### Vérifier

```bash
dig +short fatihturhan.dev
curl -I https://fatihturhan.dev
```
