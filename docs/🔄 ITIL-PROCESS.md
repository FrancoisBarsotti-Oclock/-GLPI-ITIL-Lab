# 🔄 Processus ITIL

## 🎯 Objectif

L'objectif de ce laboratoire est de mettre en œuvre un processus de gestion des incidents inspiré des bonnes pratiques **ITIL v4**, afin de structurer le support informatique, améliorer la qualité de service et assurer le respect des **Accords de Niveau de Service (SLA)**.

La démarche mise en œuvre couvre l'ensemble du cycle de vie d'un incident, depuis sa déclaration jusqu'à sa clôture, en intégrant la qualification, la priorisation, le suivi des interventions et la capitalisation des connaissances.

---

# 📖 Pourquoi ITIL ?

ITIL (Information Technology Infrastructure Library) est un référentiel international de bonnes pratiques destiné à améliorer la gestion des services informatiques.

Dans le cadre de ce projet, son utilisation permet notamment de :

- 🎫 centraliser les demandes utilisateurs ;
- 🚦 prioriser les incidents selon leur impact métier ;
- ⏱️ respecter des délais de prise en charge et de résolution (SLA) ;
- 📊 assurer une traçabilité complète des interventions ;
- 📚 capitaliser les procédures de résolution grâce à une base de connaissances.

---

# 🔄 Cycle de vie d'un incident

Le traitement des incidents suit un cycle de vie structuré permettant de garantir un suivi cohérent et reproductible.

```mermaid
flowchart LR

A["🎫 Création"] --> B["🔎 Qualification"]

B --> C["🚦 Priorisation"]

C --> D["👤 Attribution"]

D --> E["🛠️ Diagnostic"]

E --> F["✅ Résolution"]

F --> G["✔ Validation"]

G --> H["📁 Clôture"]

H --> I["📚 Base de connaissances"]
```

![Ticket history](https://github.com/FrancoisBarsotti-Oclock/-GLPI-ITIL-Lab/blob/main/docs/images/ticket-history.png)

*Figure 1 : Ticket history*

Ce processus garantit la traçabilité des interventions et facilite le traitement des incidents récurrents.

---

# 🚦 Qualification des incidents

Chaque ticket est qualifié dès sa création selon plusieurs critères.

| **Élément** | **Description** |
|----------|-------------|
| Catégorie | Domaine concerné (Réseau, Supervision, Système...) |
| Impact | Nombre d'utilisateurs ou de services affectés |
| Urgence | Rapidité d'intervention nécessaire |
| Priorité | Calculée automatiquement à partir de l'impact et de l'urgence |

Cette étape permet d'orienter rapidement le ticket vers le bon niveau de traitement.

---

# 📊 Matrice de priorisation

La priorité est déterminée à partir de la combinaison de deux critères :

- Impact
- Urgence

| Impact | Urgence | Priorité |
|----------|----------|-----------|
| Faible | Faible | 🟢 Basse |
| Faible | Élevée | 🟡 Moyenne |
| Élevé | Moyenne | 🟠 Haute |
| Élevé | Élevée | 🔴 Critique |

![Matrice de priorité](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_25-Matrice%20calcul%20de%20priorit%C3%A9.png)

*Figure 2 : Matrice de priorité*

Cette logique permet de traiter en priorité les incidents ayant le plus fort impact sur la disponibilité des services.

---

# ⏱️ Gestion des SLA

Afin de garantir une qualité de service homogène, plusieurs niveaux de Service Level Agreement (SLA) ont été définis.

Les SLA fixent :

- le délai maximal de prise en charge (**TTO – Time To Own**) ;
- le délai maximal de résolution (**TTR – Time To Resolve**).

| Priorité | TTO | TTR |
|-----------|-----|-----|
| 🔴 Critique | 30 min | 4 h |
| 🟠 Haute | 1 h | 8 h |
| 🟡 Moyenne | 4 h | 24 h |
| 🟢 Faible | 8 h | 48 h |

![Qualification](https://github.com/FrancoisBarsotti-Oclock/-GLPI-ITIL-Lab/blob/main/docs/images/category-priorities.png)

*Figure 3 : TTO + TTR*

Ces délais sont calculés selon un calendrier personnalisé intégrant :

- les horaires d'ouverture ;
- les week-ends ;
- les jours fériés.

![Calendrier](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_22-Jours%20de%20fermeture.png)

*Figure 4 : Calendrier personnalisé*

---

# ⚙️ Règles métier

Afin d'automatiser le traitement des incidents, plusieurs règles métier ont été configurées dans GLPI.

Ces règles permettent notamment :

- l'affectation automatique de certains tickets ;
- la définition automatique des priorités ;
- l'application du SLA correspondant ;
- la standardisation du traitement des incidents.

![Règles métier](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_27-Crit%C3%A8res%20et%20Actions%20des%20r%C3%A8gles.png)

*Figure 5 : Config des règles métier*

L'automatisation réduit les erreurs humaines et améliore la réactivité du support.

---

# 🎫 Exemple d'incident n°1

## Contexte

Le poste Windows ne remonte plus son inventaire dans GLPI.

### Qualification

- Catégorie : Supervision
- Impact : Faible
- Urgence : Moyenne
- Priorité : Moyenne

### Diagnostic

- Vérification de l'état du service GLPI Agent
- Contrôle de la connectivité HTTPS
- Analyse des journaux de l'agent

Cause identifiée :

Le service **GLPI Agent** était arrêté.

### Résolution

- Redémarrage du service
- Vérification du démarrage automatique
- Relance de l'inventaire

### Clôture

- Inventaire restauré
- Poste de nouveau visible dans GLPI

---

# 🌐 Exemple d'incident n°2

## Contexte

L'interface GLPI n'est plus accessible en HTTPS.

### Qualification

- Catégorie : Infrastructure
- Impact : Élevé
- Urgence : Élevée
- Priorité : Critique

### Diagnostic

- Test de connectivité réseau
- Vérification du service Apache
- Contrôle des journaux système

Cause identifiée :

Le service Apache était arrêté.

### Résolution

- Redémarrage d'Apache
- Vérification du VirtualHost HTTPS
- Validation du certificat SSL

### Clôture

- Accès HTTPS rétabli
- Plateforme GLPI de nouveau disponible

![Tickets](https://github.com/FrancoisBarsotti-Oclock/-GLPI-ITIL-Lab/blob/main/docs/images/ticket-list.png)

*Figure 6 : Liste de tickets*

---

# 📚 Base de connaissances

Afin de réduire le temps de résolution des incidents récurrents, une mini base de connaissances a été créée.

Elle regroupe des procédures simples permettant de résoudre rapidement les problèmes les plus fréquents.

Exemples :

- Vérifier le service GLPI Agent
- Redémarrer Apache
- Renouveler un bail DHCP
- Vérifier la connectivité réseau
- Contrôler les permissions de GLPI

![FAQ](https://github.com/FrancoisBarsotti-Oclock/-GLPI-ITIL-Lab/blob/main/docs/images/faq.png)

*Figure 7 : FAQ en tant que base de connaissances*

Cette démarche permet de capitaliser les connaissances techniques et d'améliorer la continuité de service.

---

# 📈 Résultats obtenus

La mise en œuvre du processus ITIL a permis :

- ✅ la centralisation des incidents ;
- ✅ une meilleure visibilité sur le parc informatique ;
- ✅ la définition de plusieurs niveaux de SLA ;
- ✅ l'automatisation d'une partie du traitement des tickets ;
- ✅ une meilleure traçabilité des interventions ;
- ✅ la création d'une base documentaire réutilisable ;
- ✅ une amélioration de la qualité du support informatique.

---

# 🚀 Perspectives d'amélioration

Le processus pourrait être enrichi par :

- l'intégration avec un annuaire LDAP ou Active Directory ;
- l'envoi automatique de notifications par courriel ;
- la création de tableaux de bord SLA ;
- la génération d'indicateurs de performance (KPI) ;
- l'intégration avec Zabbix pour l'ouverture automatique des tickets ;
- la centralisation des journaux dans un SIEM.

---

# 🏁 Conclusion

Ce laboratoire démontre qu'une solution ITSM ne se limite pas au déploiement d'un logiciel. Elle repose sur la définition de processus, la gestion des priorités, le respect des engagements de service et la traçabilité des interventions.

La mise en œuvre de GLPI selon les bonnes pratiques ITIL permet ainsi de fournir un support informatique structuré, reproductible et adapté aux besoins d'une organisation.

---
