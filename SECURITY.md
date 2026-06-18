# 🔒 SECURITY.md - Aziz-Pro v2.0

## 📌 Vue d'ensemble

Aziz-Pro est une **Progressive Web App (PWA)** de gestion de boutique utilisant **localStorage** pour le stockage des données. Ce document décrit les mesures de sécurité et les limitations.

---

## 🛡️ Mesures de Sécurité Implémentées

### 1. **Authentification Locale**
```javascript
// Authentification simple par email + nom
- Email: email@example.com
- Code: 1010 (code numérique)
- Pas de mot de passe (authentification minimale)
```

**Limitations:**
- ⚠️ Authentification très basique
- ⚠️ Pas de chiffrement de session
- ✅ Convient pour usage local/mono-utilisateur

---

### 2. **Stockage des Données**

**Emplacement:** `localStorage` du navigateur
- `ma_market_p_v3` → Produits
- `ma_market_s_v3` → Ventes
- `ma_market_e_v3` → Dépenses
- `ma_market_users_v3` → Utilisateurs
- `ma_market_audit_v3` → Logs d'audit

**Sécurité:**
- ✅ Données isolées par domaine (CORS)
- ✅ Logs d'audit enregistrés pour traçabilité
- ❌ Données en **CLAIR** (pas chiffrées)
- ❌ Accessibles via DevTools (F12)

---

### 3. **Données Sensibles**

**Données stockées:**
- Noms de produits
- Prix (achat/vente)
- Quantités en stock
- Historique des ventes
- Données clients
- Emails des utilisateurs

**⚠️ ATTENTION:** 
- Aucun chiffrement des données
- Toutes les données visibles en clair dans localStorage
- Ne pas stocker de données très sensibles (mots de passe, cartes bancaires)

---

### 4. **Contrôle d'Accès**

**Actuellement:**
- ❌ Pas de contrôle d'accès multi-utilisateur strict
- ❌ Tous les utilisateurs voient toutes les données
- ✅ Log d'audit des actions

**À améliorer:**
```
[] Contrôle d'accès par rôle (Admin, Caissier, Gestionnaire)
[] Permissions granulaires par données
[] Chiffrement des données sensibles
[] Authentification multi-facteurs
```

---

### 5. **Communication Réseau**

**Actuellement:**
- ✅ HTTPS obligatoire sur GitHub Pages
- ✅ Pas d'API backend exposée
- ✅ Pas de données transmises vers serveur externe

**Si future intégration cloud:**
```
[] HTTPS/TLS obligatoire
[] Authentification JWT ou OAuth2
[] Rate limiting sur les requêtes
[] Validation côté serveur de tous les inputs
[] Logs des accès réseau
```

---

## 🚨 Risques Identifiés

### 1. **Risque de Compromission Locale**
```
Sévérité: ⚠️ HAUTE

Si quelqu'un accède à l'ordinateur:
- Peut voir toutes les données dans DevTools
- Peut exporter/modifier les données
- Peut accéder à l'app sans authentification

Mitigation:
✅ Verrou de l'ordinateur
✅ Navigateur privé
✅ Stockage externe sécurisé
```

---

### 2. **Risque de Perte de Données**
```
Sévérité: ⚠️ HAUTE

localStorage peut être:
- Effacé par l'utilisateur (Ctrl+Shift+Del)
- Supprimé par mises à jour du navigateur
- Corrompu en cas de crash

Mitigation:
✅ Sauvegarde régulière (Export JSON)
✅ Exportation hebdomadaire recommandée
✅ Synchronisation cloud (à implémenter)
```

---

### 3. **Risque XSS (Cross-Site Scripting)**
```
Sévérité: ⚠️ MOYEN

Code React + validation minimale:
- Utilise JSX (protection automatique)
- Échappe les valeurs par défaut
- Mais validation d'input minimale

Mitigation:
✅ Validation stricte des inputs
✅ Sanitization des données importées
✅ Content Security Policy (CSP) recommandée
```

---

### 4. **Risque de Données Exposées en Clair**
```
Sévérité: ⚠️ TRÈS HAUTE

localStorage = accessible en clair:
- Pas de chiffrement
- Visible dans DevTools
- Extractible facilement

Mitigation:
❌ Pas de solution actuellement
✅ À implémenter: Chiffrement client-side
✅ Utiliser: NaCl.js ou crypto-js
```

---

## ✅ Bonnes Pratiques à Respecter

### 1. **Avant d'Utiliser l'App**
```
✅ Utiliser dans un navigateur privé
✅ Ou sur un compte utilisateur dédié
✅ Verrouiller l'ordinateur quand absent
✅ Utiliser un mot de passe Windows/Mac fort
```

### 2. **Gestion des Données**
```
✅ Exporter les données CHAQUE SEMAINE
✅ Stocker les exports sur clé USB sécurisée
✅ Sauvegarder sur cloud externe (Google Drive, OneDrive)
✅ Ne jamais laisser localStorage sans backup
```

### 3. **Partage de l'App**
```
❌ Ne pas partager sur réseau public
❌ Ne pas héberger sur serveur non-sécurisé
❌ Ne pas laisser l'app accessible à tous
✅ Utiliser lien GitHub personnel
✅ Authentification requise avant accès
```

### 4. **Mise à Jour & Maintenance**
```
✅ Vérifier régulièrement les mises à jour
✅ Tester les nouvelles versions
✅ Exporter données AVANT mise à jour
✅ Utiliser version stable
```

---

## 🔐 Roadmap Sécurité

### Phase 1 (Court terme)
- [ ] Validation stricte des inputs
- [ ] Sanitization des données importées
- [ ] Logs d'audit détaillés
- [ ] Export chiffré JSON (optionnel)

### Phase 2 (Moyen terme)
- [ ] Authentification multi-facteurs (code PIN)
- [ ] Contrôle d'accès par rôle
- [ ] Chiffrement client-side (crypto-js)
- [ ] Signature numérique des données

### Phase 3 (Long terme)
- [ ] Backend sécurisé (Node.js + Express)
- [ ] Base de données chiffrée (PostgreSQL)
- [ ] API REST avec authentification JWT
- [ ] Audit trail immuable
- [ ] Conformité RGPD/CCPA

---

## 📋 Checklist Avant Déploiement en Production

```
SÉCURITÉ:
[ ] Données chiffrées ou non-sensibles
[ ] Authentification renforcée
[ ] Logs d'audit opérationnels
[ ] Validation d'inputs stricte
[ ] Backup automatique testé
[ ] Politique de rétention de données définie

PERFORMANCE:
[ ] Service Worker actif
[ ] App fonctionne hors-ligne
[ ] Temps de chargement < 3s
[ ] PWA installable

CONFORMITÉ:
[ ] Politique de confidentialité
[ ] Conditions d'utilisation
[ ] Consentement utilisateur
[ ] Droit d'oubli implémenté
```

---

## 🚨 En Cas de Problème de Sécurité

### Données Compromises
```
1. Exporter immédiatement les données actuelles
2. Vider localStorage (Paramètres → Effacer toutes les données)
3. Recharger l'app
4. Importer dernière sauvegarde sûre
5. Vérifier audit trail pour modifications suspectes
```

### Données Perdues
```
1. Vérifier localStorage (F12 → Storage)
2. Si vide: récupérer export JSON le plus récent
3. Réimporter via "Restaurer JSON"
4. Vérifier intégrité données (montants, stocks)
```

### App Lente / Bugguée
```
1. Vider cache navigateur
2. Fermer tous les onglets
3. Redémarrer navigateur
4. Si persiste: réinstaller service worker
5. Exporter données si instable
```

---

## 📞 Support Sécurité

**Pour signaler une faille de sécurité:**
1. Ne pas partager en public
2. Documenter précisément le problème
3. Inclure: navigateur, OS, étapes de reproduction
4. Proposer correction si possible

---

## 📚 Ressources Recommandées

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [Web Security Academy](https://portswigger.net/web-security)
- [MDN Web Security](https://developer.mozilla.org/en-US/docs/Web/Security)
- [PWA Security Best Practices](https://web.dev/security/)

---

## 📝 Historique des Changements

| Date | Version | Changement |
|------|---------|-----------|
| 2026-06-06 | 1.0 | Documentation initiale |
| - | TBD | Chiffrement client-side |
| - | TBD | Backend sécurisé |
| - | TBD | Conformité RGPD |

---

## ⚖️ Avertissement Légal

```
"Aziz-Pro est fournie SANS GARANTIE de sécurité.
L'utilisateur est responsable de:
- La sécurisation de ses données
- Les sauvegardes régulières
- L'accès physique à l'ordinateur
- Le respect de la confidentialité

Ne pas utiliser pour données très sensibles
sans chiffrement supplémentaire."
```

---

**Dernière mise à jour:** 2026-06-06  
**Version:** 1.0  
**Statut:** ⚠️ PRÉ-PRODUCTION
