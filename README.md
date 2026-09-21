# istidafa4-by-moon

Bot Telegram (aiogram) pour héberger/exécuter des scripts Python.

## Fichiers
| Fichier | Rôle |
|---|---|
| `istidafa4_by_moon.py` | Le bot Telegram |
| `botconfig.py` | Page "/" : saisie de l'ID admin + token (write-once) |
| `keepalive.py` | Serveur web (`/health`) + détection du domaine + keep-alive automatique |
| `serverinfo.py` | Pays, mémoire, espace disque |
| `Dockerfile`, `docker-entrypoint.sh`, `docker-compose.yml` | Déploiement Docker |

## Première configuration (ID admin + token)
1. Déployez sans définir `TELEGRAM_BOT_TOKEN`.
2. Ouvrez l'URL du site : une petite page demande le **mot de passe**
   (`MURAD_SETUP_PASSWORD`), puis l'**ID Telegram de l'admin** et le **token**.
3. Le token est vérifié auprès de Telegram, enregistré une seule fois dans
   `DATA_DIR/murad-bot.json`, et le bot démarre immédiatement.
Si `TELEGRAM_BOT_TOKEN` est défini en variable d'environnement, la page est
désactivée. Pour changer de token : supprimez `murad-bot.json` du volume.

## Fonctions du bot
- **📊 État du serveur** (admin, ou `/status`) : pays + drapeau, IP/ville/opérateur,
  mémoire du bot et du serveur (limite du conteneur), espace disque, taille des
  scripts, scripts en cours, uptime, domaine.
- **Installation des bibliothèques en direct** : les imports du script sont
  analysés, puis la sortie de `pip` s'affiche en temps réel dans un message
  Telegram mis à jour.
- **Notifications d'arrêt** : le propriétaire est prévenu quand son script se
  termine, plante (code de sortie), ou est arrêté (par lui ou un admin). Les
  admins sont prévenus au démarrage et à l'arrêt du bot ; au redémarrage, un
  arrêt inattendu (crash/coupure) est signalé.
- **Keep-alive automatique** : ping de `https://<domaine>/health` toutes les 30 s.

## Variables d'environnement
`TELEGRAM_BOT_TOKEN`, `ADMIN_IDS`, `MURAD_SETUP_PASSWORD`, `APP_URL`, `PORT`
(auto), `KEEP_ALIVE_INTERVAL`, `DATA_DIR` (défaut `/app/data`, à monter en
volume persistant).

## Déploiement
```bash
docker compose up -d --build
```
