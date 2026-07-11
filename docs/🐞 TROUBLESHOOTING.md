# 🐞 Guide de dépannage (Troubleshooting)

## 🎯 Objectif

Ce document recense les principaux incidents rencontrés lors du déploiement et de l'exploitation de la plateforme **GLPI**.

Chaque incident est présenté selon une démarche de diagnostic structurée, comprenant :

- le symptôme observé ;
- l'analyse réalisée ;
- la cause identifiée ;
- la solution appliquée ;
- le résultat obtenu.

Cette approche permet de capitaliser les connaissances acquises et de faciliter la résolution des incidents similaires.

---

# 🖥️ Incident n°1 — Le poste Windows n'apparaît plus dans GLPI

## Symptômes

- Le poste Windows n'est plus visible dans l'inventaire.
- Les informations matérielles ne sont plus mises à jour.
- Aucun inventaire automatique n'est reçu.

## Diagnostic

- Vérification du service **GLPI Agent**.
- Contrôle de la connectivité réseau.
- Vérification de l'accès HTTPS au serveur GLPI.
- Analyse des journaux du GLPI Agent.

## Cause

Le service **GLPI Agent** était arrêté sur le poste Windows.

## Résolution

- Redémarrage du service.
- Vérification du démarrage automatique.
- Relance manuelle de l'inventaire.

## Résultat

Le poste est de nouveau remonté dans l'inventaire GLPI et les informations sont correctement synchronisées.

![Poste Win détecté](https://github.com/FrancoisBarsotti-Oclock/-GLPI-ITIL-Lab/blob/main/docs/images/detected-Win.png)

> 📸 *Figure 1 : Poste Windows détecté*

---

# 🌐 Incident n°2 — GLPI inaccessible via HTTPS

## Symptômes

- Impossible d'accéder à l'interface GLPI.
- Le navigateur retourne une erreur HTTPS.

## Diagnostic

- Vérification de la connectivité réseau.
- Contrôle du service Apache.
- Vérification de l'écoute du port TCP 443.
- Consultation des journaux Apache.

## Cause

Le service Apache était arrêté.

## Résolution

```bash
sudo systemctl restart apache2
```

Puis :

- contrôle du statut du service ;
- vérification du VirtualHost HTTPS ;
- validation du certificat SSL.

## Résultat

L'interface GLPI est de nouveau accessible en HTTPS.

![Connexion GLPI](https://github.com/FrancoisBarsotti-Oclock/-GLPI-ITIL-Lab/blob/main/docs/images/glpi-connexion.png)

> 📸 *Figure 2 : GLPI accessible après redémarrage*

---

# 🌍 Incident n°3 — Le poste Windows n'a plus accès à Internet

## Symptômes

- Navigation impossible.
- Adresse IP APIPA (`169.254.x.x`).
- Absence de passerelle.

## Diagnostic

- Vérification de la configuration IP (`ipconfig`).
- Contrôle du bail DHCP.
- Vérification de la résolution DNS.

## Cause

Le poste n'avait pas obtenu de bail DHCP valide.

## Résolution

```powershell
ipconfig /release
ipconfig /renew
ipconfig /flushdns
```

## Résultat

Le poste obtient une adresse IP valide et retrouve l'accès au réseau.

---

# 🔒 Incident n°4 — Avertissements de sécurité GLPI

## Symptômes

L'interface GLPI signale plusieurs recommandations de sécurité :

- présence du fichier `install.php` ;
- accès HTTP encore actif ;
- cookies PHP insuffisamment protégés ;
- DocumentRoot incorrect.

## Diagnostic

Analyse des recommandations affichées par GLPI.

## Cause

Configuration de sécurité incomplète après l'installation.

## Résolution

Les actions suivantes ont été réalisées :

- suppression de `install/install.php` ;
- activation du HTTPS ;
- désactivation du VirtualHost HTTP ;
- configuration de `session.cookie_secure` ;
- configuration de `session.cookie_httponly` ;
- configuration du `DocumentRoot` vers `/public`.

## Résultat

Toutes les recommandations de sécurité ont été corrigées.

![Sécurité résolue](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_17-S%C3%A9curit%C3%A9%20GLPI%20garantie.png)

> 📸 *Figure 3 : Page de vérification de sécurité GLPI.*

---

# 🔐 Incident n°5 — Permissions insuffisantes sur les fichiers GLPI

## Symptômes

- Certaines fonctionnalités de GLPI ne fonctionnent plus correctement.
- Erreurs d'accès aux fichiers.

## Diagnostic

Vérification des propriétaires et permissions :

```bash
ls -l /var/www/glpi
```

## Cause

Permissions incompatibles avec l'utilisateur utilisé par Apache.

## Résolution

```bash
sudo chown -R francois:www-data /var/www/glpi
sudo chmod -R 770 /var/www/glpi
```

## Résultat

Les accès aux fichiers sont rétablis et GLPI fonctionne normalement.

---

# 🌐 Incident n°6 — Erreur de configuration Apache

## Symptômes

Apache refuse de démarrer après modification de la configuration.

## Diagnostic

Validation de la configuration :

```bash
sudo apache2ctl configtest
```

## Cause

Erreur de syntaxe dans un fichier de configuration.

## Résolution

Correction de la configuration puis :

```bash
sudo systemctl restart apache2
```

Nouvelle validation :

```bash
sudo apache2ctl configtest
```

Résultat attendu :

```text
Syntax OK
```

## Résultat

Apache démarre normalement.

---

# 📋 Méthodologie de diagnostic

Lorsqu'un incident est détecté, la démarche suivante est appliquée :

1. Identifier le symptôme.
2. Reproduire l'incident.
3. Vérifier la connectivité réseau.
4. Contrôler les services concernés.
5. Consulter les journaux système.
6. Identifier la cause.
7. Appliquer la correction.
8. Vérifier le bon fonctionnement.
9. Documenter la résolution.

Cette approche garantit une résolution méthodique et reproductible.

---

# 📊 Outils utilisés

| Outil | Utilisation |
|--------|-------------|
| `systemctl` | Gestion des services |
| `journalctl` | Consultation des journaux système |
| `apache2ctl` | Validation de la configuration Apache |
| `ipconfig` | Diagnostic réseau Windows |
| `ping` | Vérification de la connectivité |
| `iptables` | Contrôle des règles du pare-feu |
| GLPI | Suivi des incidents |
| GLPI Agent | Inventaire automatique |

---

# 💡 Bonnes pratiques

Au cours du projet, plusieurs bonnes pratiques ont été appliquées :

- documenter systématiquement chaque incident ;
- privilégier une démarche de diagnostic avant toute correction ;
- valider le fonctionnement après chaque intervention ;
- conserver une traçabilité des actions réalisées ;
- enrichir la base de connaissances à partir des incidents récurrents.

---

# 🏁 Conclusion

Les incidents rencontrés au cours de ce laboratoire ont permis de mettre en pratique une démarche professionnelle de diagnostic et de résolution, conforme aux bonnes pratiques d'administration des systèmes.

Au-delà de la correction des anomalies, chaque intervention a été documentée afin de constituer une base de connaissances réutilisable, contribuant ainsi à l'amélioration continue du support informatique.

---
