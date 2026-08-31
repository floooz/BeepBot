# Site BeepBot

Page de téléchargement, endpoint de version et relais vers Discord.

## Déployer

Depuis ce dossier :

    npx vercel --prod

Le projet Vercel s'appelle **`beepbot-site`**. Le déploiement remplace le contenu et
**conserve l'adresse** `https://beepbot-site-flooozs-projects.vercel.app`, qui est encodée
en dur dans l'application.

### Un réglage à faire une seule fois

Dans le tableau de bord Vercel → projet **`beepbot-site`** → *Settings* → *Deployment Protection*,
passez **Vercel Authentication** sur *Disabled*. Sans ça, `version.json` et `/api/report`
renvoient une page de connexion et l'application ne peut pas les joindre.

Le projet `beepbot` (sans suffixe) est un premier essai resté vide : il peut être supprimé.

## Publier une nouvelle version

Un seul fichier à modifier, `version.json` :

```json
{
  "version": "1.1.0",
  "downloadUrl": "https://github.com/<vous>/beepbot/releases/download/v1.1.0/BeepBot-Setup-1.1.0.exe",
  "releasedAt": "2026-09-15",
  "notes": "Ce qui change dans cette version."
}
```

Puis `npx vercel --prod`. La page de téléchargement et le bouton « Vérifier maintenant »
de l'application lisent tous les deux ce fichier.

Le numéro de version doit correspondre à celui de `package.json` de l'application.

## Le relais — `api/report.js`

Reçoit les rapports de l'application et les transmet à Discord.

| Type | Salon |
|---|---|
| `bug` | webhook bugs |
| `error` | webhook bugs |
| `suggestion` | webhook suggestions |

**Les adresses des webhooks ne sont que dans ce fichier.** L'application ne les connaît pas :
elle poste sur `/api/report`. En cas d'abus, modifiez les webhooks ici et redéployez —
aucune nouvelle version de l'application n'est nécessaire.

Garde-fous : POST uniquement, 3 envois par minute et par adresse IP, message de 15 à 1800
caractères, journal tronqué à 950 caractères, caractères de contrôle retirés.
