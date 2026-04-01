# 🍁 Arrivio – Site vitrine

Site web vitrine d'Arrivio, service d'accompagnement communautaire pour les nouveaux arrivants du Grand Moncton.

## Fonctionnalités

- Page d'accueil avec présentation des services
- Formulaire de réservation → envoi automatique sur WhatsApp
- QR Code de contact direct WhatsApp
- Bouton de réservation proéminent

---

## ⚙️ Configuration avant déploiement

### 1. Mettre à jour le numéro WhatsApp

Dans `index.html`, ligne ~230, remplacez :
```js
const WHATSAPP_NUMBER = "15060000000";
```
Par votre vrai numéro au format international **sans le `+`** :
- Canada/USA : `15061234567`
- France : `33612345678`

---

## 🚀 Déploiement sur Azure Static Web Apps

### Option A — Via le portail Azure (recommandée pour débuter)

1. Connectez-vous sur [portal.azure.com](https://portal.azure.com)
2. Cherchez **"Static Web Apps"** → **Créer**
3. Remplissez :
   - **Abonnement** : votre abonnement Azure
   - **Groupe de ressources** : créez-en un (ex: `arrivio-rg`)
   - **Nom** : `arrivio-site`
   - **Région** : `Canada East` (recommandé)
   - **Source** : GitHub (ou Azure DevOps)
4. Connectez votre dépôt GitHub contenant ce projet
5. Cliquez **Vérifier + créer**
6. Azure génère automatiquement le token et le fichier de workflow GitHub Actions

### Option B — Via Azure CLI

```bash
# Installer Azure CLI si nécessaire
# https://learn.microsoft.com/fr-ca/cli/azure/install-azure-cli

az login

az group create \
  --name arrivio-rg \
  --location canadaeast

az staticwebapp create \
  --name arrivio-site \
  --resource-group arrivio-rg \
  --location canadaeast \
  --source https://github.com/VOTRE_USERNAME/VOTRE_REPO \
  --branch main \
  --app-location "/" \
  --output-location "" \
  --login-with-github
```

### Option C — Déploiement manuel (sans GitHub)

```bash
# Installer le CLI SWA
npm install -g @azure/static-web-apps-cli

# Se connecter et déployer
swa login
swa deploy ./  --env production
```

---

## 📁 Structure des fichiers

```
arrivio-site/
├── index.html                        ← Site complet (tout-en-un)
├── staticwebapp.config.json          ← Config Azure Static Web Apps
├── README.md
└── .github/
    └── workflows/
        └── azure-static-web-apps.yml ← CI/CD automatique
```

---

## 🔒 Variables secrètes GitHub

Si vous utilisez GitHub Actions (Option A ou B), ajoutez le secret dans votre dépôt :

**Settings → Secrets and variables → Actions → New repository secret**

| Nom | Valeur |
|-----|--------|
| `AZURE_STATIC_WEB_APPS_API_TOKEN` | Le token généré par Azure |

---

## 📦 Aucune dépendance à installer

Le site est 100 % HTML/CSS/JS statique. Aucun `npm install` requis.
La librairie QR Code est chargée via CDN (cdnjs.cloudflare.com).

---

*Projet de session – ADMN 6213 – Université de Moncton, Hiver 2026*
