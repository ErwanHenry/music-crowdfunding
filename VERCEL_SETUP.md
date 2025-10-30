# Configuration Vercel - Music Crowdfunding

## ✅ Statut actuel

- ✅ Repository GitHub créé : https://github.com/ErwanHenry/music-crowdfunding
- ✅ Code poussé sur GitHub
- ⚠️ Projet Vercel créé mais déploiement en erreur

## 🔧 Solution : Configuration manuelle sur Vercel

Le CLI Vercel rencontre des erreurs internes. Suivez ces étapes pour configurer manuellement :

### Étape 1 : Accéder au Dashboard Vercel

1. Allez sur https://vercel.com/dashboard
2. Connectez-vous avec votre compte

### Étape 2 : Importer le projet

Le projet **music-crowdfunding** devrait déjà apparaître dans vos projets.

Si ce n'est pas le cas :
1. Cliquez sur "Add New" → "Project"
2. Sélectionnez le repository `ErwanHenry/music-crowdfunding`
3. Cliquez sur "Import"

### Étape 3 : Configurer le projet

**Framework Preset:** Next.js (détecté automatiquement)

**Build & Output Settings:**
- Build Command: `next build`
- Output Directory: `.next` (par défaut)
- Install Command: `npm install`

**Root Directory:** `.` (racine)

### Étape 4 : Variables d'environnement (IMPORTANT)

Avant de déployer, ajoutez ces variables d'environnement :

#### Option A : Base de données Vercel Postgres (Recommandé)

1. Allez dans l'onglet "Storage" du projet
2. Cliquez sur "Create Database" → "Postgres"
3. Créez une nouvelle base de données
4. La variable `DATABASE_URL` sera automatiquement ajoutée

#### Option B : Base de données externe

Ajoutez manuellement dans Settings → Environment Variables :

```
DATABASE_URL=postgresql://user:password@host:5432/database
```

Providers recommandés (gratuits) :
- **Neon** : https://neon.tech (meilleur free tier)
- **Supabase** : https://supabase.com
- **Railway** : https://railway.app

#### Variables optionnelles (pour l'instant)

```
NEXTAUTH_URL=https://music-crowdfunding-[your-domain].vercel.app
NEXTAUTH_SECRET=[générer avec: openssl rand -base64 32]
STRIPE_SECRET_KEY=sk_test_...
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_...
```

### Étape 5 : Déployer

1. Cliquez sur "Deploy"
2. Attendez que le build se termine (2-3 minutes)
3. Votre site sera accessible sur l'URL fournie

### Étape 6 : Initialiser la base de données

Une fois déployé, initialisez la base :

```bash
# Dans votre terminal local
vercel link  # Lier au projet
vercel env pull .env.local  # Récupérer les variables d'environnement

# Créer les tables
npx prisma db push

# Ou utiliser les migrations
npx prisma migrate deploy
```

## 🎯 URLs du projet

- **Repository GitHub** : https://github.com/ErwanHenry/music-crowdfunding
- **Dashboard Vercel** : https://vercel.com/erwan-henrys-projects/music-crowdfunding
- **URL Production** : (sera généré après déploiement réussi)

## 🐛 Résolution des problèmes actuels

### Erreur "Queued" persistante
- Le projet est créé mais bloqué en queue
- Solution : Annuler les déploiements en erreur dans le dashboard
- Puis forcer un nouveau déploiement via GitHub push

### Erreur "Internal error"
- Problème temporaire avec l'API Vercel
- Solution : Attendre quelques minutes ou utiliser le dashboard web

### Prisma build error
- Ajoutez `.env.production` avec `DATABASE_URL` dummy (déjà fait ✅)
- Le vrai `DATABASE_URL` sera utilisé via les variables Vercel

## ✨ Prochaines étapes après déploiement

1. **Configurer le domaine personnalisé** (optionnel)
   - Settings → Domains
   - Ajouter votre domaine

2. **Activer Vercel Analytics**
   - Analytics → Enable

3. **Configurer Stripe**
   - Créer un compte Stripe
   - Ajouter les clés API
   - Configurer les webhooks

4. **Tester l'application**
   - Créer un compte utilisateur
   - Créer une campagne test
   - Vérifier la base de données

## 📞 Support

Si vous rencontrez des problèmes :
- Dashboard Vercel : https://vercel.com/erwan-henrys-projects/music-crowdfunding
- Logs de build : Cliquez sur un déploiement → "View Build Logs"
- Documentation Vercel : https://vercel.com/docs

---

**Statut** : Projet créé, en attente de configuration manuelle des variables d'environnement via le dashboard.
