# Huawei B612 - Gardien QoS (v1.0.0)

Script d'automatisation et de gestion de session QoS pour le routeur Huawei B612.

---

## ⚡ Installation Automatique en 1 Seule Commande

Pour installer le script, configurer vos identifiants du routeur et activer automatiquement le service Systemd en arrière-plan, **copiez-collez l'intégralité du bloc suivant dans votre terminal Linux** :

```bash
wget -q https://github.com/ZeedeFi/D-bridage-du-QoS-de-Orange-Mali-sur-un-Routeur-Huawei/archive/refs/tags/v1.0.0.zip -O /tmp/v1.0.0.zip && unzip -q -o /tmp/v1.0.0.zip -d /tmp/ && cd /tmp/D-bridage-du-QoS-de-Orange-Mali-sur-un-Routeur-Huawei-1.0.0 && chmod +x setup.sh && sudo ./setup.sh
```

> **Note :** Pendant l'exécution de `setup.sh`, le programme vous demandera d'entrer l'adresse IP et le mot de passe administrateur de votre routeur Huawei. Il se chargera ensuite d'installer les dépendances et de créer le service Systemd tout seul.

---

## ⚙️ Configuration Automatique du Service Systemd

Si vous souhaitez créer ou réinstaller le service Systemd manuellement sans réinterroger vos identifiants, vous pouvez exécuter directement ce script monolithique en copier-coller :

```bash
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
EOF' && sudo systemctl daemon-reload && sudo systemctl enable --now huawei-qos.service
```

---

## 🔊 Comment ajouter/personnaliser les sons et alertes audio ?

Si votre script utilise des alertes sonores ou des notifications audio lors des changements d'état réseau :

1. Préparez vos fichiers audio au format `.mp3` ou `.wav`.
2. Déplacez vos fichiers de son dans le dossier dédié `/opt/huawei_qos_guardian/sounds/` en exécutant :

```bash
# Exemple pour copier vos sons dans le répertoire du script :
sudo mkdir -p /opt/huawei_qos_guardian/sounds
sudo cp /chemin/vers/votre_son.mp3 /opt/huawei_qos_guardian/sounds/alert.mp3
```

3. Assurez-vous que le fichier `.env` ou `qos_guardian.py` pointe vers le bon fichier son dans `/opt/huawei_qos_guardian/sounds/`.

---

## 🛠️ Commandes de Gestion et Suivi du Service

```bash
# Vérifier si le service tourne correctement
sudo systemctl status huawei-qos.service

# Suivre les logs en temps réel (alertes, réinitialisations, détections)
sudo journalctl -u huawei-qos.service -f

# Redémarrer le service
sudo systemctl restart huawei-qos.service

# Arrêter temporairement le service
sudo systemctl stop huawei-qos.service
