# OPENVAS-1
Install OPENVAS sous Docker

Installation :
Install ca-certificates, curl and gnupg Debian/Ubuntu packages
sudo apt install ca-certificates curl gnupg
Désinstaller les packages Ubuntu en conflit
 sudo apt remove $pkg
Configurer le référentiel Docker
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
Installer les packages Docker Ubuntu
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin
Ajoutez l'utilisateur actuel au groupe Docker et appliquez les modifications du groupe pour l'environnement shell actuel
sudo usermod -aG docker $USER && su $USER
Créer un répertoire de téléchargement
export DOWNLOAD_DIR=$HOME/greenbone-community-container && mkdir -p $DOWNLOAD_DIR
Téléchargement du fichier docker-compose
cd $DOWNLOAD_DIR && curl -f -L https://greenbone.github.io/docs/latest/_static/docker-compose-22.4.yml -o docker-compose.yml
Téléchargement des conteneurs de la communauté Greenbone
docker compose -f $DOWNLOAD_DIR/docker-compose.yml -p greenbone-community-edition pull
Démarrage des conteneurs communautaires Greenbone
docker compose -f $DOWNLOAD_DIR/docker-compose.yml -p greenbone-community-edition up –d
Mise à jour du mot de passe de l'utilisateur administrateur
docker compose -f $DOWNLOAD_DIR/docker-compose.yml -p greenbone-community-edition \
    exec -u gvmd gvmd gvmd --user=admin --new-password='<password>'
Ouverture de Greenbone Security Assistant dans le navigateur
xdg-open "http://127.0.0.1:9392" 2>/dev/null >/dev/null &

