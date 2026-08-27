# Huawei B612 - Gardien QoS (v1.0.0)

Script d'automatisation et d'optimisation QoS pour le routeur Huawei B612.

---

## ⚡ Installation Automatique en 1 Clic (Copier - Coller)

Avant de lancer la commande, adapte tes identifiants de routeur sur la 2ème ligne (`ROUTER_IP` et `ROUTER_PASSWORD`). 

Ouvre ton terminal sur ton système Linux et **colle l'intégralité du bloc suivant** :

```bash
# === PARAMÈTRES DU ROUTEUR (À MODIFIER SI BESOIN) ===
IP_ROUTEUR="192.168.8.1"
PASS_ROUTEUR="VotreMotDePasseAdmin"

# === INSTALLATION ET DÉPLOIEMENT AUTOMATIQUE ===
echo "--> Téléchargement et extraction de la v1.0.0..."
wget -q [https://github.com/ZeedeFi/D-bridage-du-QoS-de-Orange-Mali-sur-un-Routeur-Huawei/archive/refs/tags/v1.0.0.zip](https://github.com/ZeedeFi/D-bridage-du-QoS-de-Orange-Mali-sur-un-Routeur-Huawei/archive/refs/tags/v1.0.0.zip) -O /tmp/v1.0.0.zip
unzip -q -o /tmp/v1.0.0.zip -d /tmp/
cd /tmp/D-bridage-du-QoS-de-Orange-Mali-sur-un-Routeur-Huawei-1.0.0

echo "--> Préparation des répertoires système..."
sudo mkdir -p /opt/huawei_qos_guardian

echo "--> Création du fichier de configuration .env..."
sudo bash -c "cat << EOF > /opt/huawei_qos_guardian/.env
ROUTER_IP=${IP_ROUTEUR}
ROUTER_PASSWORD=${PASS_ROUTEUR}
EOF"

echo "--> Copie du script..."
sudo cp qos_guardian.py /opt/huawei_qos_guardian/

echo "--> Configuration de l'environnement virtuel et des dépendances..."
sudo python3 -m venv /opt/huawei_qos_guardian/venv
sudo /opt/huawei_qos_guardian/venv/bin/pip install -q python-dotenv requests

echo "--> Création et configuration du service systemd..."
sudo bash -c 'cat << EOF > /etc/systemd/system/huawei-qos.service
[Unit]
Description=Gardien QoS Huawei B612
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
User=root
Group=root
WorkingDirectory=/opt/huawei_qos_guardian
ExecStart=/opt/huawei_qos_guardian/venv/bin/python /opt/huawei_qos_guardian/qos_guardian.py
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
EOF'

echo "--> Lancement du service..."
sudo systemctl daemon-reload
sudo systemctl enable --now huawei-qos.service

echo "--> Nettoyage des fichiers temporaires..."
rm -f /tmp/v1.0.0.zip
rm -rf /tmp/D-bridage-du-QoS-de-Orange-Mali-sur-un-Routeur-Huawei-1.0.0

echo "✅ FINI ! Le script tourne maintenant en arrière-plan !"
