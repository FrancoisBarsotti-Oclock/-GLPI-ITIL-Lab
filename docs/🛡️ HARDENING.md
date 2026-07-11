# 🛡️ Durcissement de l’infrastructure

## 🎯 Objectif

Le durcissement du serveur GLPI vise à réduire sa surface d’attaque, protéger les données traitées par l’application et limiter les risques liés aux configurations par défaut.

Les mesures mises en œuvre concernent plusieurs niveaux :

- le système Debian ;
- le serveur web Apache ;
- les communications HTTPS ;
- les permissions du système de fichiers ;
- les sessions PHP ;
- l’application GLPI ;
- le filtrage réseau.

> Ce laboratoire reproduit une démarche de sécurisation adaptée à un environnement de test. Certaines mesures, notamment l’utilisation d’un certificat auto-signé, devraient être renforcées dans un environnement de production.

---

## 🔎 Risques identifiés

Avant le durcissement, plusieurs risques de sécurité pouvaient être présents :

| **Risque** | **Conséquence potentielle** |
|---|---|
| Comptes GLPI avec mots de passe par défaut | Accès non autorisé à l’application |
| Accès HTTP non chiffré | Interception des identifiants et des sessions |
| Fichier d’installation conservé | Réexécution ou détournement du processus d’installation |
| Permissions trop permissives | Modification ou lecture non autorisée des fichiers |
| Exposition de l’arborescence GLPI | Accès à des fichiers non destinés au public |
| Sessions PHP insuffisamment protégées | Vol ou détournement de cookies de session |
| Services réseau non filtrés | Augmentation de la surface d’attaque |
| Configuration Apache incomplète | Fuite d’informations ou comportement imprévisible |

---

# 🔐 Mesures de sécurité appliquées

## 🛡️ 1. Sécurisation des comptes GLPI

Les mots de passe par défaut de l’ensemble des comptes intégrés à GLPI ont été modifiés immédiatement après l’installation.

![modif mdp](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_12-Modif%20du%20mdp%20du%20super%20utilisateur.png)

> *Figure 1 — Comptes GLPI sécurisés après modification des mots de passe par défaut.*

Les nouveaux mots de passe ont été définis selon des principes de robustesse :

- longueur suffisante ;
- diversité des caractères ;
- absence d’informations personnelles ;
- mot de passe unique pour chaque compte ;
- suppression de l’usage des identifiants par défaut.

Cette opération permet de réduire le risque d’accès non autorisé à l’interface d’administration.

---

## 2. Suppression du fichier d’installation

Une fois l’installation terminée, le fichier suivant a été supprimé :

```bash
/var/www/glpi/install/install.php
```

Cette mesure empêche la réutilisation de l’assistant d’installation après la mise en production de l’application.

```bash
sudo rm /var/www/glpi/install/install.php
```

---

## 🛡️ 3. Restriction du répertoire exposé par Apache

Le `DocumentRoot` du VirtualHost Apache a été configuré afin de pointer uniquement vers le répertoire public de GLPI :

```apache
DocumentRoot /var/www/glpi/public
```
![Apache](https://github.com/FrancoisBarsotti-Oclock/-GLPI-ITIL-Lab/blob/main/docs/images/Apache-On.png)

> *Figure 2 — Validation de la configuration Apache après les modifications de sécurité.*

Cette configuration évite l’exposition directe des répertoires internes de l’application, notamment ceux contenant les fichiers de configuration, les bibliothèques et les données applicatives.

Exemple de configuration :

```apache
<VirtualHost *:443>
    ServerName glpi.local
    DocumentRoot /var/www/glpi/public

    <Directory /var/www/glpi/public>
        Require all granted
        AllowOverride All
        Options FollowSymLinks
    </Directory>
</VirtualHost>
```

---

## 4. Configuration du nom global Apache

Un nom de serveur global a été défini afin de supprimer les avertissements Apache et de garantir une configuration explicite :

```apache
ServerName glpi.local
```

La configuration a ensuite été validée avec :

```bash
sudo apache2ctl configtest
```

Résultat attendu :

```text
Syntax OK
```

---

## 🌐 5. Chiffrement des communications HTTPS

L’accès à l’interface GLPI a été configuré en HTTPS afin de protéger :

- les identifiants d’authentification ;
- les cookies de session ;
- les données des tickets ;
- les informations d’inventaire ;
- les échanges d’administration.

Dans le cadre du laboratoire, un certificat auto-signé a été généré et associé au VirtualHost Apache sur le port `443`.

Le module SSL a été activé avec :

```bash
sudo a2enmod ssl
```

Le VirtualHost HTTPS a ensuite été activé :

```bash
sudo a2ensite glpi-ssl.conf
sudo systemctl reload apache2
```

![HTTPS](https://github.com/FrancoisBarsotti-Oclock/-GLPI-ITIL-Lab/blob/main/docs/images/https.png)

> *Figure 3 : Certificat HTTPS*

> Un certificat auto-signé est adapté à un laboratoire isolé. En production, il conviendrait d’utiliser un certificat délivré par une autorité de certification interne ou publique.

---

## 6. Désactivation de l’accès HTTP

Le VirtualHost HTTP a été désactivé afin de ne conserver que l’accès sécurisé en HTTPS.

```bash
sudo a2dissite 000-default.conf
sudo systemctl reload apache2
```

Cette mesure empêche l’utilisation involontaire d’une connexion non chiffrée.

Une alternative possible en production serait de conserver le port `80` uniquement afin de rediriger automatiquement les requêtes vers HTTPS.

---

## 7. Protection des sessions PHP

Les paramètres de sécurité suivants ont été activés dans la configuration PHP :

```ini
session.cookie_secure = On
session.cookie_httponly = On
```

### `session.cookie_secure`

Le cookie de session est transmis uniquement via une connexion HTTPS.

### `session.cookie_httponly`

Le cookie n’est pas accessible depuis du code JavaScript exécuté dans le navigateur, ce qui réduit les risques de vol de session en cas de faille XSS.

Après modification, Apache a été redémarré :

```bash
sudo systemctl restart apache2
```

---

## 📂 8. Gestion des propriétaires et des permissions

Les propriétaires et groupes des fichiers GLPI ont été ajustés afin de permettre leur utilisation par Apache tout en limitant l’accès aux autres utilisateurs du système.

Le groupe `www-data`, utilisé par Apache, a été associé aux fichiers nécessaires.

Exemple :

```bash
sudo chown -R francois:www-data /var/www/glpi
```

Les permissions ont été définies afin que :

- le propriétaire dispose des droits nécessaires ;
- le groupe Apache puisse accéder aux fichiers requis ;
- les autres utilisateurs ne disposent d’aucun accès inutile.

Exemple :

```bash
sudo chmod -R 770 /var/www/glpi
```

> En production, les permissions doivent être définies de manière plus granulaire. L’application ne doit disposer de droits d’écriture que sur les répertoires qui en ont réellement besoin.

Le principe appliqué reste celui du **moindre privilège**.

---

## 🔥 9. Filtrage réseau avec iptables

Le serveur Debian a été protégé à l’aide de règles `iptables` afin de limiter les connexions entrantes aux services nécessaires.

![iptables](https://github.com/FrancoisBarsotti-Oclock/-GLPI-ITIL-Lab/blob/main/docs/images/iptables.png)

> *Figure 4 — Politique de filtrage réseau appliquée au serveur Debian.*

Les flux autorisés sont principalement :

| Port | Protocole | Utilisation |
|---:|---|---|
| `22` | TCP | Administration SSH |
| `443` | TCP | Accès HTTPS à GLPI |
| `80` | TCP | Désactivé ou réservé à une éventuelle redirection |

Exemple de politique restrictive :

```bash
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT
```

Autorisation des connexions déjà établies :

```bash
sudo iptables -A INPUT \
  -m conntrack --ctstate ESTABLISHED,RELATED \
  -j ACCEPT
```

Autorisation de l’interface locale :

```bash
sudo iptables -A INPUT -i lo -j ACCEPT
```

Autorisation de HTTPS :

```bash
sudo iptables -A INPUT \
  -p tcp --dport 443 \
  -j ACCEPT
```

Autorisation de SSH depuis le réseau d’administration :

```bash
sudo iptables -A INPUT \
  -p tcp \
  -s 10.0.0.0/24 \
  --dport 22 \
  -j ACCEPT
```

> Les règles présentées doivent être adaptées à l’architecture réelle afin d’éviter toute perte d’accès distant.

---

## 🔒 10. Vérification des alertes GLPI

Après l’installation et la sécurisation, les avertissements affichés dans l’interface GLPI ont été analysés et corrigés.

Les contrôles ont notamment porté sur :

- la présence du fichier d’installation ;
- la configuration du répertoire public ;
- la sécurité des cookies PHP ;
- les permissions des fichiers ;
- l’utilisation de HTTPS ;
- la configuration du serveur web.

![Plus d'erreur](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_17-S%C3%A9curit%C3%A9%20GLPI%20garantie.png)

> *Figure 5 — Vérification des recommandations de sécurité de GLPI après le durcissement.*

L’objectif était de ne conserver aucune alerte de sécurité connue dans l’interface d’administration.

---

# ✅ Contrôles de validation

Les vérifications suivantes ont été réalisées après le durcissement :

| Contrôle | Résultat attendu |
|---|---|
| Test de la configuration Apache | `Syntax OK` |
| Accès à GLPI en HTTP | Refusé ou redirigé |
| Accès à GLPI en HTTPS | Fonctionnel |
| État du service Apache | Actif |
| Écoute du port `443` | Confirmée |
| Présence de `install.php` | Fichier supprimé |
| Cookies `Secure` et `HttpOnly` | Activés |
| Alertes de sécurité GLPI | Corrigées |
| Accès GLPI Agent | Fonctionnel |
| Règles iptables | Actives |

Commandes de contrôle utilisées :

```bash
sudo systemctl status apache2
sudo apache2ctl configtest
sudo ss -lntp
sudo iptables -L -n -v
```
![GLPI opérationnelle](https://github.com/FrancoisBarsotti-Oclock/-GLPI-ITIL-Lab/blob/main/docs/images/glpi-dashboard.png)

> *Figure 6 — Validation finale du bon fonctionnement de l'infrastructure sécurisée.*

---

# 📊 Synthèse du durcissement

| Domaine | Mesure appliquée | État |
|---|---|:---:|
| Comptes GLPI | Modification des mots de passe par défaut | ✅ |
| Installation | Suppression de `install.php` | ✅ |
| Apache | `DocumentRoot` limité à `/public` | ✅ |
| Apache | Définition du `ServerName` | ✅ |
| Chiffrement | Accès HTTPS | ✅ |
| HTTP | VirtualHost désactivé | ✅ |
| PHP | Cookie `Secure` | ✅ |
| PHP | Cookie `HttpOnly` | ✅ |
| Fichiers | Propriétaires et permissions ajustés | ✅ |
| Réseau | Filtrage avec `iptables` | ✅ |
| GLPI | Alertes de sécurité corrigées | ✅ |

---

# ⚠️ Limites du laboratoire

Certaines mesures restent adaptées à un environnement pédagogique et devraient être améliorées pour une mise en production :

- certificat HTTPS auto-signé ;
- serveur web et base de données hébergés sur la même VM ;
- absence d’authentification centralisée ;
- absence de reverse proxy ;
- absence de haute disponibilité ;
- sauvegardes non automatisées ;
- journalisation non centralisée ;
- règles de pare-feu limitées à l’environnement du laboratoire.

---

# 🚀 Améliorations futures

Les évolutions suivantes pourraient renforcer davantage la sécurité :

- utiliser un certificat délivré par une autorité de certification ;
- intégrer GLPI à un annuaire LDAP ou Active Directory ;
- restreindre SSH par clés publiques uniquement ;
- désactiver l’authentification SSH par mot de passe ;
- mettre en place Fail2ban ;
- automatiser les mises à jour de sécurité ;
- sauvegarder régulièrement la base MariaDB et les fichiers GLPI ;
- déployer un reverse proxy ;
- centraliser les journaux dans un SIEM ;
- superviser le serveur avec Zabbix ;
- réaliser des audits de vulnérabilités réguliers.

---

# 🏁 Résultat

Le serveur GLPI dispose désormais d’un socle de sécurité cohérent avec les besoins du laboratoire :

- communications chiffrées ;
- comptes par défaut sécurisés ;
- fichiers sensibles protégés ;
- accès réseau filtrés ;
- sessions PHP renforcées ;
- surface d’exposition Apache réduite ;
- alertes de sécurité GLPI corrigées.

Cette démarche permet de démontrer que la mise en place d’un service ne se limite pas à son fonctionnement technique, mais inclut également sa sécurisation, son contrôle et la validation des mesures appliquées.

---
