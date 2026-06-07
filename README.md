# Rapport d'analyse de sécurité mobile

## Informations générales

| Champ | Valeur |
|---|---|
| Date | 07/06/2025 |
| Cible | UnCrackable L1 |
| Outils | BeVigil v2.1.0 · Yaazhini v1.3.2 |
| Périmètre | APK pédagogique — environnement isolé |

---

## Résumé exécutif

L'analyse de UnCrackable L1 a permis d'identifier plusieurs indices d'exposition classiques d'une application Android non sécurisée. Les constats couvrent principalement le stockage de données sensibles en clair, des configurations à risque dans le Manifest, et des communications potentiellement non chiffrées. Niveau de risque global : **Medium**.

---

## Top 5 constats

### FIND-001 — Clé API exposée dans le code
- **Sévérité** : High
- **Source** : BeVigil
- **Impact** : Accès non autorisé aux services backend
- **Remédiation** : Stocker via Android Keystore, ne jamais hardcoder
- **OWASP** : MASVS-STORAGE-1

### FIND-002 — Communication HTTP non chiffrée
- **Sévérité** : Medium
- **Source** : Yaazhini
- **Impact** : Interception possible des données en transit
- **Remédiation** : Forcer TLS sur toutes les communications
- **OWASP** : MASVS-NETWORK-1

### FIND-003 — `allowBackup=true` dans AndroidManifest
- **Sévérité** : Medium
- **Source** : BeVigil + Yaazhini
- **Impact** : Extraction des données applicatives via sauvegarde ADB
- **Remédiation** : Désactiver `allowBackup` en production
- **OWASP** : MASVS-STORAGE-4

### FIND-004 — Mode debug activé (`debuggable=true`)
- **Sévérité** : Medium
- **Source** : Yaazhini
- **Impact** : Accès au processus de l'app via ADB, lecture mémoire possible
- **Remédiation** : Désactiver en build de production
- **OWASP** : MASVS-CODE-2

### FIND-005 — Permissions excessives déclarées
- **Sévérité** : Low
- **Source** : Yaazhini
- **Impact** : Surface d'attaque élargie, données utilisateur accessibles inutilement
- **Remédiation** : Appliquer le principe de moindre privilège
- **OWASP** : MASVS-PLATFORM-1

---

## Recommandations prioritaires

1. Désactiver `debuggable` et `allowBackup` dans tous les builds de production.
2. Migrer les secrets et clés API vers Android Keystore ou une solution côté serveur.
3. Forcer HTTPS sur l'ensemble des communications réseau de l'application.

---

## Clôture

- Aucun secret réel exposé dans ce rapport.
- Analyse réalisée dans le cadre d'un lab de formation sur cible autorisée uniquement.
