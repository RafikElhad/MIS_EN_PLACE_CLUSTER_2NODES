TP Linux - mise en place d'un cluster de 2 nodes 



<img width="510" alt="S3" src="https://github.com/user-attachments/assets/7fcc8133-8ac4-4da7-87bd-a46441d2aaf9" />



Ce fichier de configuration Vagrant définit une machine virtuelle nommée 'webserver' basée sur Ubuntu 22.04. 
La machine a un timeout de démarrage de 1000 secondes, utilise un réseau privé avec l'adresse IP 192.168.33.10, et 
redirige le port 80 de la machine virtuelle vers le port 8080 de l'hôte.
Un dossier local est synchronisé avec '/var/www/html' sur la machine virtuelle. 
La configuration VirtualBox alloue 1 Go de RAM et 1 CPU à la machine, avec l'interface graphique désactivée.



<img width="579" alt="S4" src="https://github.com/user-attachments/assets/aba78324-5d13-4542-950d-e86b3eee07b3" />



Ce fichier de configuration Vagrant définit une machine virtuelle appelée "dbserver" basée sur Ubuntu 22.04. 
La machine a un port forwardé (port 80 sur la machine virtuelle vers le port 8086 sur l'hôte) et utilise un réseau privé avec l'adresse IP 192.168.33.11. 
Un dossier local est synchronisé avec "/var/www/html" sur la machine virtuelle. La configuration VirtualBox alloue 1 Go de RAM et 1 CPU à la machine, avec l'interface graphique désactivée.

De plus, un script Shell est exécuté pour provisionner la machine : 
il met à jour les paquets, installe MySQL, démarre et active le service MySQL, 
puis crée une base de données nommée "test" et un utilisateur MySQL avec des privilèges spécifiques.



<img width="549" alt="dev1" src="https://github.com/user-attachments/assets/cc3f9dd0-a597-4066-9ce2-54f20c693064" />




Ce fichier montre la sortie d'une commande Vagrant qui démarre deux machines virtuelles, "webserver" et "dbserver", avec le fournisseur VirtualBox.

webserver : La machine est reprise d'un état suspendu et démarre avec l'adresse SSH 127.0.0.1:2222. Elle est déjà provisionnée, 
donc aucun nouveau provisionnement n'est nécessaire.

dbserver : La machine est configurée avec des ports forwardés (port 80 vers 8086 et port 22 vers 2200). Elle démarre avec l'adresse SSH 127.0.0.1:2200. 

En résumé, les deux machines sont démarrées avec succès



<img width="558" alt="dev2" src="https://github.com/user-attachments/assets/039661f4-fbba-421d-90e0-7e68addc9713" />


Ce fichier montre la suite de l'exécution de Vagrant pour la machine 'webserver'. 
Les dossiers partagés sont montés correctement, et la machine est déjà provisionnée.

Ensuite, je me connecte en SSH à la machine 'webserver' qui fonctionne sous Ubuntu 22.04.5 LTS. 
Je passe en mode superutilisateur avec sudo su pour avoir les droits d'administration.



<img width="607" alt="dev3" src="https://github.com/user-attachments/assets/24084181-f0a7-4fce-84c7-c3a33e8893d6" />



Dans cette capture, je me connecte en SSH à la machine virtuelle 'dbserver' depuis un terminal Windows. 
La machine fonctionne sous Ubuntu 22.04.3 LTS. 
La machine a deux adresses IP : une pour l'interface réseau NAT (10.0.2.15) et une pour le réseau privé (192.168.33.11).
Je passe ensuite en mode superutilisateur avec sudo su pour obtenir les droits d'administration.





<img width="400" alt="dev9" src="https://github.com/user-attachments/assets/f78c5207-88c0-4c97-a5c7-9abc8730c389" />




On a créé un utilisateur MySQL nommé appuser avec l'adresse IP 192.168.33.10 et le mot de passe rafik. 
Cela permet à cet utilisateur d'accéder à la base de données MySQL depuis cette adresse IP spécifique.





<img width="382" alt="dev10" src="https://github.com/user-attachments/assets/f72b5ce3-193b-4fae-96f2-b0d5b4f407c1" />



On a accordé tous les droits (GRANT ALL) sur la base de données adminappdb à l'utilisateur appuser depuis l'adresse IP 192.168.33.10. 
Cela permet à cet utilisateur de gérer entièrement cette base de données.




<img width="414" alt="V7" src="https://github.com/user-attachments/assets/e37fd427-5d48-4b8d-a994-6ff8f3e2745b" />



J'ai utilisé la commande ping pour vérifier la connectivité entre mon ordinateur et l'adresse IP 192.168.33.10. 
Toutes les requêtes ont reçu une réponse en moins de 1 ms, ce qui signifie que la connexion est rapide et stable, 
avec aucune perte de paquets. Cela confirme que l'adresse IP est accessible depuis mon ordinateur.





<img width="249" alt="dev11" src="https://github.com/user-attachments/assets/4b727ad4-7029-4cab-8a2e-6130959643ee" />



J'ai exécuté la commande SHOW DATABASES; dans MySQL pour lister toutes les bases de données disponibles. 
Les bases de données affichées sont adminappdb, information_schema, mysql, performance_schema, sys, et test. 
Cela montre que la base de données adminappdb existe et est prête à être utilisée.




<img width="550" alt="dev5" src="https://github.com/user-attachments/assets/9ef98df8-54df-4fc2-979b-554c2e707174" />



Dans cette capture, je suis connecté en tant que superutilisateur sur la machine virtuelle 'webserver'. 
J'ai cloné un dépôt Git nommé 'admin-app' depuis GitHub dans le répertoire /usr/src/app. 
Le clonage a réussi, et le dépôt contient 237 objets avec une taille totale de 893.95 KiB. 
Après le clonage, je liste le contenu du répertoire pour vérifier que le dossier 'admin-app' a bien été créé, 
puis je me déplace dans ce dossier avec la commande cd admin-app/



<img width="905" alt="dev4" src="https://github.com/user-attachments/assets/6e97c934-458e-4d51-bc4d-321cff6ecbbb" />



puis on accede à notre application app-admin pour voir ce qu'il y a à l'interieur




<img width="896" alt="dev6" src="https://github.com/user-attachments/assets/544144c2-63a2-4a6d-883c-242ec1e0685d" />



On a utilisé la commande mvn clean install -DskipTests pour construire le projet en nettoyant les fichiers générés précédemment (clean), 
en compilant le code et en créant le package (install), tout en sautant l'exécution des tests (-DskipTests). 






<img width="877" alt="dev7" src="https://github.com/user-attachments/assets/95280a61-e753-407a-9b80-37f57d38567c" />




La commande mvn clean install -DskipTests a été exécutée avec succès, sautant les tests et construisant le projet. 
Ensuite, la commande mvn spring-boot:run a été utilisée pour démarrer l'application Spring Boot, et 
la sortie a été redirigée vers un fichier log.txt pour enregistrer les logs. 
Le projet a été correctement compilé et empaqueté dans un fichier JAR.





<img width="637" alt="V9" src="https://github.com/user-attachments/assets/9de413ec-2b82-4efd-bbe8-45b1eaffc011" />




J'ai envoyé une requête POST à l'URL http://192.168.33.10:8080/roles pour créer un nouveau rôle. 
Le corps de la requête était en JSON et contenait les informations suivantes : "id": 1 et "nom": "Ngor". 
La requête a réussi avec un statut 201 Created, ce qui signifie que le rôle a été créé avec succès. 






<img width="612" alt="V8" src="https://github.com/user-attachments/assets/7f9f5557-5d5c-4bc5-ba34-760f8c3e2e41" />



La réponse reçue après la requête est au format JSON et contient les informations suivantes : "id": 1 et "nom": "Ngor". 
Cela indique que le rôle avec l'identifiant 1 et le nom "Ngor" a été récupéré avec succès. 
La réponse est bien formatée et montre que l'opération a fonctionné comme prévu.












