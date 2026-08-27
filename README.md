# Huawei B612 - Gardien QoS (Version Beta)

Outil privé d'automatisation et de gestion de session QoS pour le routeur Huawei B612.

---

## 🚀 Téléchargement rapide (Version v1.0.0)

Pour installer directement la version **v1.0.0** archivée sans utiliser Git :

```bash
# 1. Télécharger l'archive ZIP officielle de la v1.0.0
wget [https://github.com/ZeedeFi/D-bridage-du-QoS-de-Orange-Mali-sur-un-Routeur-Huawei/archive/refs/tags/v1.0.0.zip](https://github.com/ZeedeFi/D-bridage-du-QoS-de-Orange-Mali-sur-un-Routeur-Huawei/archive/refs/tags/v1.0.0.zip)

# 2. Décompresser et accéder au dossier
unzip v1.0.0.zip
cd D-bridage-du-QoS-de-Orange-Mali-sur-un-Routeur-Huawei-1.0.0
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
