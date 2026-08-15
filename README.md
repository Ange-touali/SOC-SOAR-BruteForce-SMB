# Captures de preuve

Sélection de captures issues du laboratoire SOC/SOAR.

## Splunk
- `01_event_4625_bruteforce_smb.jpg` — investigation des échecs Windows 4625 depuis `192.168.56.102`.
- `02_pic_1429_evenements_0246.png` — pic de `1 429` événements observé à `02:46`.
- `03_alerte_soc_4625_v2.png` — configuration de l'alerte `SOC-4625-BruteForce-SMB-v2`.

## Shuffle SOAR
- `04_condition_ip_privee_startswith_192_168.png` — condition `source_ip startswith 192.168.`.
- `05_workflow_shuffle_soar.jpg` — workflow Webhook → condition → Discord / VirusTotal.
- `06_run_ip_publique_8_8_8_8.jpg` — run public avec `source_ip: 8.8.8.8` et `count: 25`.

## Discord
- `07_alerte_ip_publique_virustotal.jpg` — notification publique enrichie VirusTotal (`Harmless: 53`).
- `08_alerte_ip_privee_routage_direct.jpg` — notification privée `192.168.56.102 / 1429`, enrichissement ignoré.

## Dashboard
- `09_dashboard_soc_final.png` — dashboard Splunk final avec `1 916` échecs d'authentification SMB.

> Les secrets, clés API, mots de passe et URLs complètes de Webhook ne doivent jamais être publiés.
