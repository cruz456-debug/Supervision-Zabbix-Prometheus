# Supervision-Zabbix-Prometheus
Ce projet répond à une problématique majeure de l'administration système : la fragmentation des données. Plutôt que de jongler entre plusieurs outils, j'ai conçu une interface unique capable de surveiller à la fois la disponibilité des services (monitoring d'état) et la santé matérielle précise (métriques haute fidélité).

# Supervision Unifiée : Zabbix, Prometheus & Grafana
**Projet Académique - Licence 3 ETSE**
**Etablissement : ISM Dakar**

## Présentation du projet
L'objectif de ce projet était de centraliser le monitoring d'état et les métriques de performance au sein d'une interface de visualisation unique. Ce "lab" simule un environnement de production où la visibilité sur l'état des services et les ressources système est critique.

## Architecture de la Solution
La solution repose sur la complémentarité de trois outils majeurs :
* **Zabbix :** Gestion de la disponibilité (Up/Down), inventaire matériel et alertes.
* **Prometheus :** Collecte de métriques haute fidélité (séries temporelles) via Node Exporter.
* **Grafana :** Moteur de visualisation centralisant les données de Zabbix et Prometheus sur des tableaux de bord dynamiques.

---

## Réalisation Technique

### 1. Déploiement de Prometheus et Node Exporter
Prometheus a été installé manuellement sur l'environnement VMware pour garantir la stabilité. Pour la collecte des données système (CPU, RAM, Disque), l'agent **Node Exporter** a été déployé :

```bash
# Installation du collecteur de métriques système
sudo apt install prometheus-node-exporter -y

# Vérification du flux de données sur le port 9100
curl http://localhost:9100/metrics
```

### 2. Configuration de Zabbix 6.0 LTS
Le serveur Zabbix a été déployé sous Ubuntu 22.04 pour surveiller la disponibilité des services critiques (comme Postfix ou Dovecot).

```bash
# Lancement et activation des services Zabbix
sudo systemctl enable --now zabbix-server zabbix-agent apache2
```

### 3. Intégration et Visualisation sous Grafana
Grafana sert de "vitrine" décisionnelle. L'intégration unifiée a nécessité la configuration de deux sources de données (Data Sources) :
* **Source Zabbix :** Connexion via l'API JSON-RPC (`http://localhost/zabbix/api_jsonrpc.php`).
* **Source Prometheus :** Connexion locale sur le port `9090`.

---

## Sécurisation de la Plateforme
Pour garantir l'intégrité des données de monitoring, des mesures de restriction ont été appliquées :
* **Filtrage réseau :** Configuration des accès via UFW sur les ports 3000 (Grafana), 9090 (Prometheus) et 10050 (Zabbix).
* **Contrôle d'accès :** Authentification renforcée pour l'accès aux interfaces web et isolation des services.

---

## Bilan Technique
Cette architecture permet une visibilité totale sur l'infrastructure. Zabbix identifie immédiatement les pannes ("ce qui est cassé"), tandis que Prometheus permet une analyse fine du comportement système ("comment le système réagit"). L'utilisation de Grafana permet de superposer ces deux mondes pour une prise de décision rapide.

---

## Document technique

[Cliquer ici pour télécharger le rapport de supervision complet (PDF)](zabbix+grafana+prometheus%28Dylan%29.pdf)
```
