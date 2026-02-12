# securite-mobile-lab1x
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
