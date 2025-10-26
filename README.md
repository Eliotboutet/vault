# 🧱 Vaultwarden – Docker Compose Setup

Ce projet permet de déployer **Vaultwarden**, une implémentation légère et open-source du gestionnaire de mots de passe **Bitwarden**, à l’aide de **Docker Compose**.

---

## 🚀 Démarrage rapide

### 1. Crée le dossier de ton service
```bash
mkdir -p ~/Services/Vaultwarden
cd ~/Services/Vaultwarden
```

### 2. Copie le fichier `docker-compose.yml`
```yaml
services:
  vaultwarden:
    image: vaultwarden/server:latest
    container_name: vaultwarden
    restart: always
    ports:
      - "8080:80"
      - "3012:3012"
    environment:
      WEBSOCKET_ENABLED: "true"
    volumes:
      - /vw-data/:/data/
```

### 3. Lance le service
```bash
docker compose up -d
```

### 4. Accède à l’interface web
Ouvre ton navigateur et rends-toi sur :  
👉 [http://localhost:8080](http://localhost:8080)

---

## ⚙️ Détails du fichier `docker-compose.yml`

### 🧩 `services`
Définit les conteneurs à exécuter. Ici, un seul service : **vaultwarden**.

### 🐳 `image: vaultwarden/server:latest`
Spécifie l’image Docker officielle utilisée pour exécuter Vaultwarden.  
Le tag `latest` récupère la version la plus récente.

### 🏷️ `container_name: vaultwarden`
Nom personnalisé du conteneur Docker, pour faciliter la gestion (ex. `docker ps`).

### 🔁 `restart: always`
Assure que le conteneur redémarre automatiquement :
- après un crash,  
- après un redémarrage du serveur.

### 🔌 `ports`
Mappe les ports du conteneur vers ceux de l’hôte :
- `8080:80` → accès à l’interface web de Vaultwarden.  
- `3012:3012` → port utilisé pour les WebSockets (notifications temps réel).

### 🌐 `environment`
Définit les variables d’environnement :
- `WEBSOCKET_ENABLED=true` → active le support WebSocket pour une meilleure synchronisation entre clients.

### 💾 `volumes`
Monte un dossier de ton hôte vers le conteneur :
- `/vw-data/:/data/` → stocke de façon persistante les données de Vaultwarden (utilisateurs, mots de passe, fichiers…).

> ⚠️ **Important** : assure-toi que `/vw-data/` existe et est accessible par Docker.

---

## 🧹 Gestion du conteneur

- **Arrêter le service :**
  ```bash
  docker compose down
  ```

- **Voir les logs :**
  ```bash
  docker compose logs -f
  ```

- **Mettre à jour Vaultwarden :**
  ```bash
  docker compose pull
  docker compose up -d
  ```

---

## 🔒 Notes de sécurité

- Configure un proxy HTTPS (ex. **Traefik**, **Caddy**, ou **Nginx**) pour sécuriser l’accès.  
- Pense à définir des variables comme `ADMIN_TOKEN` pour protéger l’accès administrateur.  
- Sauvegarde régulièrement le contenu de `/vw-data/`.

---

