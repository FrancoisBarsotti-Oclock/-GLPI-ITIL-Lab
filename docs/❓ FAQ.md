# 📚 Base de connaissances (FAQ)

## 🎯 Objectif

Cette base de connaissances regroupe les procédures de résolution des incidents les plus courants rencontrés lors du déploiement et de l'exploitation de la plateforme **GLPI**.

Son objectif est de :

- réduire le temps de résolution des incidents ;
- standardiser les procédures de dépannage ;
- faciliter le travail des techniciens du support ;
- assurer la continuité de service ;
- capitaliser les connaissances acquises au fil des interventions.

Cette démarche s'inscrit dans les bonnes pratiques **ITIL**, où chaque incident résolu contribue à enrichir la documentation technique.

---

# 🖥️ Le poste Windows n'apparaît plus dans GLPI

## Symptômes

- Le poste n'est plus visible dans l'inventaire.
- Aucune remontée automatique n'est effectuée.
- Les informations matérielles ne sont plus mises à jour.

## Vérifications

Sur le poste Windows :

- Vérifier que le service **GLPI Agent** est démarré.
- Vérifier la connectivité réseau avec le serveur GLPI.
- Vérifier l'accès HTTPS au serveur.
- Contrôler les journaux du GLPI Agent.

## Résolution

- Redémarrer le service **GLPI Agent**.
- Vérifier son démarrage automatique.
- Relancer un inventaire manuel.
- Contrôler la remontée des informations dans GLPI.

![Win déctecté](https://github.com/FrancoisBarsotti-Oclock/-GLPI-ITIL-Lab/blob/main/docs/images/detected-Win.png)

*Figure 1 : Poste Windows détecté*

---

# 🌐 GLPI est inaccessible via HTTPS

## Symptômes

- Le navigateur affiche une erreur de connexion.
- L'interface GLPI ne s'ouvre plus.

## Vérifications

Sur le serveur Debian :

```bash
sudo systemctl status apache2
```

Contrôler également :

- le port HTTPS (443) ;
- le certificat SSL ;
- les journaux Apache.

## Résolution

```bash
sudo systemctl restart apache2
```

Puis vérifier :

- le bon démarrage du service ;
- l'accès HTTPS depuis un poste client.

![Apache On](https://github.com/FrancoisBarsotti-Oclock/-GLPI-ITIL-Lab/blob/main/docs/images/Apache-On.png)

*Figure 2 : Apache activé*

---

# 🌍 Le poste Windows n'a plus accès à Internet

## Symptômes

- Impossible de naviguer sur Internet.
- Les tests `ping` échouent.
- Une adresse APIPA (`169.254.x.x`) est attribuée.

## Vérifications

Sous Windows :

```powershell
ipconfig
```

Vérifier :

- l'adresse IP ;
- la passerelle par défaut ;
- les serveurs DNS.

## Résolution

```powershell
ipconfig /release
ipconfig /renew
ipconfig /flushdns
```

Puis vérifier la connectivité avec :

```powershell
ping 8.8.8.8
ping google.com
```

---

# 🔐 Impossible de se connecter à GLPI

## Symptômes

- Échec d'authentification.
- Mot de passe refusé.

## Vérifications

- Vérifier le nom d'utilisateur.
- Vérifier le mot de passe.
- Contrôler que le compte est actif.

## Résolution

Réinitialiser le mot de passe depuis un compte administrateur si nécessaire.

---

# 🔒 Le navigateur affiche un avertissement HTTPS

## Symptômes

Le navigateur indique que le certificat n'est pas reconnu.

## Cause

Le laboratoire utilise un certificat SSL auto-signé.

## Résolution

Ajouter une exception de sécurité dans le navigateur ou installer le certificat dans le magasin de certificats de confiance.

> En production, un certificat délivré par une autorité de certification doit être utilisé.

---

# 🛡️ Vérifier les règles de pare-feu

## Vérification

```bash
sudo iptables -L -n -v
```

Contrôler notamment :

- le port 443 ;
- le port 22 ;
- les politiques INPUT.

![iptables](https://github.com/FrancoisBarsotti-Oclock/-GLPI-ITIL-Lab/blob/main/docs/images/iptables.png)

*Figure 3 : Vérification des règles de pare-feu*

---

# 📂 Vérifier les permissions GLPI

## Vérification

```bash
ls -l /var/www/glpi
```

Contrôler :

- le propriétaire ;
- le groupe ;
- les permissions.

Si nécessaire :

```bash
sudo chown -R francois:www-data /var/www/glpi
sudo chmod -R 770 /var/www/glpi
```

---

# 📊 Vérifier les services essentiels

## Apache

```bash
sudo systemctl status apache2
```

## MariaDB

```bash
sudo systemctl status mariadb
```

Les deux services doivent être à l'état :

```
active (running)
```

---

# 📋 Checklist de diagnostic rapide

Avant toute intervention :

☐ Vérifier la connectivité réseau

☐ Vérifier l'adresse IP

☐ Vérifier les services système

☐ Vérifier HTTPS

☐ Vérifier les journaux

☐ Vérifier les permissions

☐ Vérifier les règles iptables

☐ Vérifier GLPI Agent

☐ Vérifier les alertes GLPI

---

# 💡 Bonnes pratiques

Afin de maintenir une plateforme GLPI fiable et sécurisée, il est recommandé de :

- documenter chaque incident résolu ;
- maintenir la base de connaissances à jour ;
- appliquer régulièrement les mises à jour de sécurité ;
- sauvegarder la base de données MariaDB ;
- surveiller l'état des services Apache et MariaDB ;
- contrôler périodiquement les règles du pare-feu ;
- renouveler les certificats HTTPS avant leur expiration.

---

# 🏁 Conclusion

Cette base de connaissances constitue un support opérationnel destiné à faciliter le traitement des incidents les plus fréquents.

En documentant les procédures de résolution, elle contribue à améliorer la qualité du support, à réduire le temps d'intervention et à assurer une meilleure continuité de service conformément aux bonnes pratiques **ITIL**.

---
