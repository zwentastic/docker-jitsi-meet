# Prepare Raspi (Docker + Build Environment)

```sh
sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get install -y git curl nano jq libffi-dev libssl-dev python3 python3-pip
sudo apt-get remove -y python-configparser

curl -sSL -o install.sh https://get.docker.com
chmod +x install.sh
sudo ./install.sh
sudo usermod -aG docker pi
sudo reboot
sudo pip3 install docker-compose

cd ~
git clone -b arm git@github.com:zwentastic/docker-jitsi-meet.git
cd docker-jitsi-meet
sudo make FORCE_REBUILD=1

# for buiding single service, use:
# sudo make FORCE_REBUILD=1 JITSI_SERVICES=jvb

cp env.example .env
mkdir -p ~/.jitsi-meet-cfg/{web/letsencrypt,transcripts,prosody,jicofo,jvb}

docker-compose up 

# for reset config and restarting docker services, use:
# docker-compose rm && sudo rm -rf ~/.jitsi-meet-cfg && mkdir -p ~/.jitsi-meet-cfg/{web/letsencrypt,transcripts,prosody,jicofo,jvb} && docker-compose up -d && docker-compose logs -f jvb

```


