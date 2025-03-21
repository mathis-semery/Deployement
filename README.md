# Deployement

# Déploiement d'un Site Web sous Debian (LAMP)

Ce guide décrit les étapes à suivre pour configurer une machine virtuelle Debian 12 afin d'héberger un site web en utilisant la stack LAMP (Linux, Apache, MySQL, PHP).

# Prérequis

Une machine virtuelle Debian 12 sous Hyper-V.
Accès root à la machine.
Un client SSH (ex: PuTTY).
Un client FTP (ex: FileZilla).

# Partie 1 : Prise en Main de Linux

1. Accéder à la Machine Virtuelle

# Se connecter  :

Utilisateur : ? (votre nom utilisateur configuré)
Mot de passe :? (votre mot de passe )
Passer en mode root :
su -
Mot de passe : ? ( votre mot de passe Root , Admin)

# 2. Commandes de Base

Affiche le répertoire actuel
ls

Liste les fichiers du répertoire
cd

Change de répertoire
mkdir

Crée un dossier
rm

Supprime un fichier
cp

Copie un fichier
mv

# Partie 2 : Installation et Configuration du Serveur SSH

# 1. Installation de SSH

sudo apt update
sudo apt install openssh-server -y

Vérifier l'état du service SSH :
systemctl status ssh

Activer SSH au démarrage :
systemctl enable ssh

# 2. Connexion via Putty

Lancer Putty et entrer l'adresse IP de la VM.
( pour trouver l'ip de la Vm taper " ip a " ) L'ip se trouve sur la ligne  " inet  Ip brd Ip scope global dynamic "

Se connecter avec l'utilisateur debian.

# Partie 3 : Installation et Configuration du Serveur FTP

# 1. Installation du Serveur FTP (vsftpd)

sudo apt install vsftpd -y

Vérifier et démarrer le service :

systemctl status vsftpd
systemctl start vsftpd

# 2. Configuration de vsftpd

Modifier /etc/vsftpd.conf :
sudo nano /etc/vsftpd.conf

Modifier ces lignes :
anonymous_enable=NO
local_enable=YES
write_enable=YES

Redémarrer le service :
systemctl restart vsftpd

# Partie 4 : Installation et Configuration de LAMP

# 1. Installer Apache, MySQL et PHP

sudo apt install apache2 mysql-server php libapache2-mod-php php-mysql -y

Vérifier les services :

systemctl status apache2
systemctl status mysql

# Activer MySQL :

sudo mysql_secure_installation

# 2. Création d'un Virtual Host

sudo nano /etc/apache2/sites-available/monsite.local.conf

Ajouter :

<VirtualHost *:80>
    ServerName monsite.local
    ServerAlias www.monsite.local
    DocumentRoot /var/www/monsite.local
    <Directory /var/www/monsite.local>
        AllowOverride All
        Require all granted
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

# Activer le site et recharger Apache :

sudo a2ensite monsite.local
sudo systemctl reload apache2

# 3. Modifier le Fichier Hosts sur Windows

Modifier C:\Windows\System32\drivers\etc\hosts :

192.168.X.X monsite.local
192.168.X.X www.monsite.local

# Partie 5 : Transfert du Site Web

# 1. Transfert via FTP

Se connecter avec FileZilla et envoyer les fichiers dans /var/www/monsite.local/.

# 2. Transfert via Git

Installer Git :

sudo apt install git -y

# Cloner un projet :

git clone https://github.com/utilisateur/repository.git /var/www/monsite.local/

# Attribuer les bons droits :

sudo chown -R www-data:www-data /var/www/monsite.local
sudo chmod -R 755 /var/www/monsite.local

# Conclusion

Votre site web est maintenant déployé et accessible via monsite.local. Vous pouvez continuer à le configurer selon vos besoins !

