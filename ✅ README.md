# 🗂️ GLPI-ITIL-Lab

<p align="center">

![GLPI](https://img.shields.io/badge/GLPI-10.x-blue?style=for-the-badge)
![Debian](https://img.shields.io/badge/Debian-12-A81D33?style=for-the-badge&logo=debian)
![Apache](https://img.shields.io/badge/Apache-2.4-D22128?style=for-the-badge&logo=apache)
![MariaDB](https://img.shields.io/badge/MariaDB-10.x-003545?style=for-the-badge&logo=mariadb)
![PHP](https://img.shields.io/badge/PHP-8.x-777BB4?style=for-the-badge&logo=php)
![HTTPS](https://img.shields.io/badge/HTTPS-Secured-success?style=for-the-badge)
![ITIL](https://img.shields.io/badge/ITIL-v4-blueviolet?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

</p>

---

# 📖 Présentation

**GLPI-ITIL-Lab** est un laboratoire de virtualisation ayant pour objectif de déployer une plateforme **IT Service Management (ITSM)** basée sur **GLPI**, afin de mettre en œuvre un processus complet de gestion des incidents conforme aux bonnes pratiques **ITIL**.

Ce projet ne se limite pas à l'installation de GLPI : il couvre également le **durcissement du serveur**, la **gestion des équipements**, la **mise en place des SLA**, la **priorisation des incidents**, la **traçabilité des interventions** ainsi que la **capitalisation des connaissances** grâce à une base documentaire.

L'ensemble de l'infrastructure est déployé dans un environnement virtualisé **Proxmox VE**, reproduisant un contexte réaliste d'administration d'infrastructures sécurisées.

---

# ✨ Fonctionnalités

- 🖥️ Déploiement d'un serveur GLPI sous Debian 12
- 🌐 Architecture LAMP (Apache, MariaDB, PHP)
- 🔒 Accès sécurisé en HTTPS
- 🛡️ Durcissement du serveur selon les bonnes pratiques
- 🔥 Protection réseau avec iptables
- 💻 Inventaire automatique des postes via GLPI Agent
- 🎫 Gestion complète des incidents
- 🚦 Priorisation des tickets
- ⏱️ Mise en œuvre des SLA
- 📚 Base de connaissances (FAQ)
- 📝 Historique complet des interventions

---

# 🎯 Objectifs pédagogiques

Ce laboratoire avait pour objectifs de :

- Déployer une solution ITSM professionnelle
- Mettre en œuvre les bonnes pratiques ITIL
- Sécuriser une infrastructure Linux
- Centraliser la gestion des incidents
- Automatiser l'inventaire du parc informatique
- Développer une documentation technique exploitable

---

# 🏢 Contexte

*(à rédiger ensemble ensuite)*

---

# 🏗️ Architecture

*(capture Proxmox + schéma réseau)*

---

# 🛠️ Technologies utilisées

| Domaine | Solution |
|----------|----------|
| Hyperviseur | Proxmox VE |
| Serveur | Debian 12 |
| Web | Apache |
| Base de données | MariaDB |
| Langage | PHP |
| ITSM | GLPI |
| Inventaire | GLPI Agent |
| Poste client | Windows 10 |
| Sécurité | HTTPS • iptables |

---

# 🔒 Sécurisation

*(avec renvoi vers HARDENING.md)*

---

# 🎫 Gestion des incidents

*(avec renvoi vers ITIL-PROCESS.md)*

---

# 📸 Captures d'écran

*(toutes les captures)*

---

# 🐞 Dépannage

*(renvoi vers TROUBLESHOOTING.md)*

---

# 📚 Documentation

| Document | Description |
|-----------|-------------|
| ARCHITECTURE.md | Architecture de l'infrastructure |
| HARDENING.md | Mesures de sécurité |
| ITIL-PROCESS.md | Gestion des incidents |
| TROUBLESHOOTING.md | Résolution des incidents |
| FAQ.md | Base de connaissances |

---

# 🚀 Améliorations futures

- Authentification LDAP
- Notifications par e-mail
- Sauvegarde automatisée de MariaDB
- Déploiement avec Ansible
- Supervision via Zabbix
- Tableau de bord SLA

---

# 🎓 Compétences démontrées

| Domaine | Compétence |
|----------|------------|
| Linux | ✅ |
| Apache | ✅ |
| MariaDB | ✅ |
| PHP | ✅ |
| HTTPS | ✅ |
| Firewall | ✅ |
| ITIL | ✅ |
| GLPI | ✅ |
| SLA | ✅ |
| Documentation | ✅ |
| Gestion des incidents | ✅ |

---

# 👤 Auteur

**François Barsotti**

Administrateur d'Infrastructures Sécurisées (titre RNCP en cours de validation)

Passionné par l'administration système, la cybersécurité et l'automatisation des infrastructures.

---
