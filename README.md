# 🔄 GitLab → GitHub Commit Sync

Synchronise automatiquement tes commits GitLab vers GitHub **sans copier le moindre code**.  
Chaque commit fantôme embarque les métadonnées récupérées via l'API GitLab :

```
fix: handle null pointer on user login

[gitlab-sync]
  project  : mon-equipe/backend-api
  commit   : a3f9c12
  changes  : +47 / -12 lines across 3 file(s)
```

---

## ⚙️ Installation

### Structure du repo

```
gitlab-sync/
├── .github/
│   └── workflows/
│       └── sync-gitlab-commits.yml
└── README.md
```

### Secrets à configurer

**GitHub → Settings → Secrets and variables → Actions**

| Secret         | Valeur                                                        |
|----------------|---------------------------------------------------------------|
| `GITLAB_TOKEN` | Token GitLab, scope **`read_api`** uniquement                 |
| `GITLAB_USER`  | Ton **email** GitLab (ex: `prenom.nom@job.fr`)                |
| `GITLAB_URL`   | *(optionnel)* URL si instance perso : `https://gitlab.job.fr` |
| `GH_PAT`       | Personal Access Token GitHub, scope **`repo`**                |
| `GIT_NAME`     | Ton nom affiché (ex: `Jean Dupont`)                           |
| `GIT_EMAIL`    | Ton **email principal GitHub** ⚠️ (doit matcher le compte)    |

---

## 🕰️ Mode rétroactif

Pour rattraper les commits des dernières semaines, déclenche manuellement depuis  
**Actions → Sync GitLab Commits → Run workflow** et entre le nombre de jours :

| Valeur | Effet                          |
|--------|--------------------------------|
| `0`    | Sync normal (depuis le dernier |
| `14`   | Remonte 2 semaines en arrière  |
| `21`   | Remonte 3 semaines en arrière  |
| `90`   | Remonte 3 mois en arrière      |

---

## 🔒 Ce qui est récupéré (sans code)

| Donnée                        | Source API GitLab           |
|-------------------------------|-----------------------------|
| Message du commit             | `title`                     |
| Date exacte                   | `authored_date`             |
| Projet + namespace            | `name`, `namespace`         |
| SHA court                     | `short_id`                  |
| Lignes ajoutées / supprimées  | `stats.additions/deletions` |
| Nombre de fichiers modifiés   | `stats.total`               |

Aucun diff, aucun fichier, aucun contenu de code n'est transmis.

---

## ❓ Les contributions n'apparaissent pas ?

1. `GIT_EMAIL` doit être l'email **principal** de ton compte GitHub
2. **GitHub → Settings → Emails** : vérifie que l'email n'est pas masqué
3. Sur ton profil GitHub, active **"Private contributions"** pour les repos privés
