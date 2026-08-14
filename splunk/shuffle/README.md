# Shuffle SOAR

## Objectif

Shuffle reçoit les alertes générées par Splunk via Webhook et automatise le traitement de l'adresse IP source.

## Données reçues

Le payload contient notamment :

- `search_name`
- `source_ip`
- `count`

## Logique du workflow

Splunk Alert → Webhook → Shuffle → Condition sur `source_ip`

Deux traitements sont appliqués :

- **IP privée (`192.168.x.x`)** → notification directe vers Discord.
- **IP publique** → enrichissement VirusTotal → notification Discord.

## Tests réalisés

### IP publique

Test avec `8.8.8.8` :

`Webhook → Condition → VirusTotal → Discord`

L'appel VirusTotal retourne un statut HTTP 200 et la notification Discord contient les informations de réputation.

### IP privée

Test avec `192.168.56.102` :

`Webhook → Condition → Discord`

VirusTotal est volontairement ignoré afin d'éviter un enrichissement inutile d'une adresse interne.

## Sécurité

Les clés API, tokens et URLs complètes des Webhooks ne sont pas publiés dans ce dépôt.
