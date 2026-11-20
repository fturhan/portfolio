# Portfolio — fatihturhan.dev

CV en ligne de Fatih Turhan : une seule page statique (`index.html`, Tailwind et AOS chargés
depuis un CDN), sans étape de build.

Sections : projets en cours, expériences, compétences, formations, langues.

## Voir le site en local

```bash
python3 -m http.server 8765
```

Puis ouvrir http://localhost:8765.

## Déploiement

Le site est publié par **GitLab Pages** : chaque push sur `main` lance le job `pages` de
`.gitlab-ci.yml`, qui copie les fichiers du dépôt dans `public/`.

### Rendre le site public (une fois)

Par défaut, GitLab Pages peut être réservé aux membres du projet.
Dans GitLab : **Settings → General → Visibility, project features, permissions → Pages** :
choisir **Everyone**.

### Brancher le domaine fatihturhan.dev (une fois)

1. **GitLab** : **Deploy → Pages → Domains & settings → New domain**.
   - Ajouter `fatihturhan.dev`, laisser « Automatic certificate management using Let's
     Encrypt » activé.
   - Noter l'enregistrement TXT de vérification affiché.
   - Recommencer pour `www.fatihturhan.dev`.
2. **OVH** (Web Cloud → Noms de domaine → fatihturhan.dev) :
   - Onglet **Redirection** : supprimer la redirection de `fatihturhan.dev` vers
     `www.fatihturhan.dev`. Elle crée l'enregistrement `A 213.186.33.5` et le TXT
     `"1|www.fatihturhan.dev"`, qui doivent disparaître.
   - Onglet **Zone DNS** :

     | Sous-domaine | Type | Cible |
     |---|---|---|
     | *(vide)* | A | `35.185.44.232` |
     | *(vide)* | AAAA | `2600:1901:0:7b8a::` |
     | `www` | CNAME | `fturhan.gitlab.io.` |
     | `_gitlab-pages-verification-code` | TXT | `gitlab-pages-verification-code=…` (valeur donnée par GitLab) |
     | `_gitlab-pages-verification-code.www` | TXT | `gitlab-pages-verification-code=…` (valeur donnée par GitLab pour www) |

     Supprimer l'ancien `A` de `www` (`57.128.25.21`). Ne pas toucher aux enregistrements MX
     et SPF (messagerie OVH) ni à `ecoroulez`.
3. Une fois la propagation DNS faite (de quelques minutes à quelques heures), cliquer sur
   **Verify** pour chaque domaine dans GitLab. Le certificat Let's Encrypt arrive ensuite
   automatiquement.

Le domaine `.dev` impose le HTTPS dans tous les navigateurs : le site n'est joignable qu'une
fois le certificat émis par GitLab.

### Vérifier

```bash
dig +short fatihturhan.dev
curl -I https://fatihturhan.dev
```
