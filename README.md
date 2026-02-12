# securite-mobile-lab1x
## L'objectif de ce laboratoire est de mettre en place un environnement contrôlé pour l'audit de sécurité mobile en utilisant la distribution spécialisée Mobexler. Cette plateforme regroupe l'ensemble des outils nécessaires à l'analyse statique et dynamique, ainsi qu'aux tests d'intrusion sur les applications mobiles.

Dans un premier temps, nous avons procédé à la configuration et à la validation de l'infrastructure réseau. Cette étape est cruciale pour garantir que la machine d'attaque peut communiquer avec les serveurs distants, résoudre les noms de domaine (DNS) et interagir avec les terminaux cibles.

Par la suite, nous avons mis en œuvre des mesures de persistance et de sécurité de l'environnement via la création de points de restauration (Snapshots). Enfin, nous avons initié la communication avec le périphérique mobile via le protocole ADB (Android Debug Bridge), étape préalable indispensable à l'extraction de données et à l'analyse du comportement applicatif en temps réel.
<img width="1000" height="999" alt="image" src="https://github.com/user-attachments/assets/aa078533-a861-494d-a224-b221b17a91ef" />
# Connexion à Mobexler
Après le lancement de la machine virtuelle, on se connecte à Mobexler avec les identifiants fournis.


# Capture d'écran de l'interface de commande affichant la configuration réseau via la commande ip a. On identifie l'interface réseau active ens33 avec l'adresse IP locale 192.168.10.129 (masque /24). Cette adresse sera utilisée comme point de terminaison pour l'écoute des connexions entrantes (Reverse Shell) ou pour la communication avec le terminal mobile dans le cadre du lab

# Démarrage de Mobexler
Une fois connecté, Mobexler démarre normalement et l’environnement est prêt à l’utilisation.
<img width="1095" height="812" alt="image" src="https://github.com/user-attachments/assets/712a1369-cebc-40c4-b9ae-b3549b57b653" />

# Vérifier les adresses IP
Identification de l'adresse IP de l'attaquant : La commande ip address révèle que la machine hôte utilise l'IP 192.168.10.129. Cette information est cruciale pour configurer les payloads (comme un fichier APK modifié avec MSFvenom) afin qu'ils pointent vers la bonne machine de contrôle.
<img width="1000" height="999" alt="image" src="https://github.com/user-attachments/assets/82f539a7-8677-4bb6-a5a0-4a1b72a351d9" />
# Vérifier route par défaut
<img width="950" height="424" alt="image" src="https://github.com/user-attachments/assets/8019f49a-1f5c-4a54-801d-f1083ea64f68" />

Affichage de la table de routage IP via la commande ip route. On observe que la route par défaut (default) passe par la passerelle 192.168.10.2 via l'interface ens33. On note également la présence d'une interface docker0 sur le sous-réseau 172.17.0.0/16, actuellement en état 'linkdown', ce qui indique que l'environnement Docker est configuré mais qu'aucun conteneur n'est actif sur ce segment au moment du test
# Tester Internet
<img width="958" height="236" alt="image" src="https://github.com/user-attachments/assets/a4df3fb5-dff8-4e15-a345-e21405e90f31" />


Validation de l'accès à Internet et de la résolution réseau. La commande ping 8.8.8.8 (DNS Google) confirme que la machine dispose d'une connexion active vers l'extérieur. Les temps de réponse stables (environ 24-25 ms) garantissent que les outils de sécurité pourront télécharger des dépendances ou communiquer avec des serveurs distants si nécessaire lors de l'analyse des applications mobiles
<img width="934" height="277" alt="image" src="https://github.com/user-attachments/assets/b9b9cebe-6d2a-405c-8e67-2f40b09f6372" />
Validation du service de résolution de noms (DNS). La commande ping vers le nom de domaine 'https://www.google.com/url?sa=E&source=gmail&q=google.com' réussit, prouvant que la machine peut traduire les noms de domaine en adresses IP
# Étape 5 — Créer le snapshot “CLEAN” (baseline)

<img width="985" height="605" alt="image" src="https://github.com/user-attachments/assets/adbda281-ec69-45c2-bb83-d34f62fad0a3" />
Utilisation de la fonctionnalité 'Snapshot' de VMware pour sauvegarder l'état actuel de la machine virtuelle. Cette étape de sécurité permet de figer une configuration stable (Clean Baseline) avant de manipuler des malwares mobiles ou de modifier des fichiers système critiques.
<img width="1278" height="499" alt="image" src="https://github.com/user-attachments/assets/1100555e-bb63-42ad-b55c-76f96e5c7d4a" />
Configuration du snapshot nommé 'CLEAN_BASELINE_TP1'. La description précise que l'importation est réussie, que les interfaces réseau (NAT + HostOnly) sont opérationnelles, et que le service ADB (Android Debug Bridge) est prêt pour les tests sur terminaux mobiles. Ce point de sauvegarde garantit une réinitialisation rapide en cas d'erreur durant le lab

# Étape 6 — Préparer la cible Android (choisir 1 option)
l’outil ADB est bien installé et utilisable dans Mobexler.
Résultats attendus:
<img width="1025" height="335" alt="image" src="https://github.com/user-attachments/assets/589771b5-ac71-4af3-a747-80422c1e7c3d" />
# Dépannage
unauthorized : accepter la popup RSA sur le téléphone.
rien n’apparaît :
vérifier USB passthrough
relancer ADB 
<img width="975" height="295" alt="image" src="https://github.com/user-attachments/assets/14c5476a-071d-4ab1-b741-9aa5356fcc81" />
Réinitialisation du démon ADB (Android Debug Bridge). La commande adb kill-server suivie de adb start-server permet de s'assurer que le service de communication avec les appareils Android tourne sur une instance propre. Le terminal confirme que le démon a démarré avec succès sur le port TCP 5037
