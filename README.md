# SOC-SOAR-BruteForce-SMB

Laboratoire SOC/SOAR consacré à la détection et à l'automatisation de la réponse à une attaque **Brute Force SMB**.

## Objectif

Construire une chaîne de traitement de bout en bout :

**Simulation SMB → Journaux Windows → Splunk → Détection SPL → Alerte V2 → Webhook → Shuffle SOAR → Décision IP privée/publique → VirusTotal si nécessaire → Discord → Dashboard SOC**

Le scénario s'appuie principalement sur les échecs d'authentification Windows **Event ID 4625**.

## Technologies

- Splunk Enterprise — SIEM, recherche SPL, alerte et dashboard
- Windows — machine cible et journaux de sécurité
- Kali Linux — simulation de l'activité SMB dans le laboratoire
- Shuffle SOAR — orchestration et logique conditionnelle
- VirusTotal — enrichissement des adresses IP publiques
- Discord — notification automatisée

## Scénario observé

La simulation génère des échecs d'authentification SMB sur la machine Windows. Splunk collecte les événements 4625 et permet d'identifier notamment :

- une adresse IP source ;
- le compte ciblé ;
- le Logon Type 3 ;
- le volume d'échecs d'authentification.

Dans le test principal du laboratoire, l'adresse privée `192.168.56.102` a généré **1 429** échecs d'authentification.

## Détection Splunk

La règle utilisée dans le projet est nommée :

`SOC-4625-BruteForce-SMB-v2`

Elle exploite les événements Windows 4625, extrait l'adresse réseau source et compte les événements par IP.

Voir : [`splunk/detection-bruteforce-smb.spl`](splunk/detection-bruteforce-smb.spl)

## Automatisation SOAR

L'alerte Splunk transmet les données à Shuffle via Webhook.

Le workflow applique ensuite une logique conditionnelle :

- **IP privée (`192.168.x.x`)** → notification Discord directe ;
- **IP publique** → enrichissement VirusTotal → notification Discord.

### Validation IP publique

Test avec `8.8.8.8` :

- Failed Logons : 25
- Severity : HIGH
- VirusTotal : Malicious 0 / Suspicious 0 / Harmless 53
- Notification Discord générée après enrichissement.

### Validation IP privée

Test réel avec `192.168.56.102` :

- Failed Logons : 1 429
- Severity : HIGH
- Adresse reconnue comme locale/privée
- Enrichissement VirusTotal volontairement ignoré
- Notification Discord directe.

## Dashboard SOC

Le dashboard Splunk **SOC - Dashboard Brute Force SMB** regroupe notamment :

- le nombre total d'échecs Windows 4625 ;
- l'évolution temporelle des échecs SMB ;
- les principales adresses IP sources ;
- les comptes les plus ciblés.

## Structure du dépôt

```text
SOC-SOAR-BruteForce-SMB/
├── README.md
├── .gitignore
├── SECURITY.md
├── docs/
├── splunk/
├── shuffle/
├── screenshots/
│   ├── splunk/
│   ├── shuffle/
│   ├── discord/
│   └── dashboard/
└── presentation/
```

## Sécurité

Ce dépôt ne contient volontairement **aucun secret** :

- aucune clé API VirusTotal ;
- aucune URL complète de Webhook Shuffle ;
- aucun Webhook Discord ;
- aucun token ;
- aucun mot de passe ou identifiant.

Les valeurs sensibles doivent être configurées localement et ne jamais être commitées.

## Documentation

Le rapport technique complet est disponible dans le dossier [`docs/`](docs/).

## Statut

Projet de laboratoire pédagogique — SOC / Blue Team — 2026.
