# Alertmanager webhook for Telegram on Openshift

One way to get the chat ID
==========================
1) Add bot on channel
2) Send message on this channel with @botname
3) Access access the link https://api.telegram.org/botXXX:YYYY/getUpdates (xxx:yyyy botID)

Another way to get the chat ID
==============================
1) Access https://web.telegram.org/
2) Click to specific chat to the left
3) At the url, you can get the chat ID


> make sure set proper username and password when you exposing your app on internet


Setup Telegram receiver 
========================

```
oc new-project telegram
# Build and deploy
oc new-app --name=telegram \
    https://github.com/ralvares/alertmanager-webhook-telegram.git \
    -e FLASK_ENV=development \
    -e APP_FILE=flaskAlert.py \
    -e TELEGRAM_BOTTOKEN="9999999999:AAAAAA-AAAAAA-AAAAA...." \
    -e BASIC_AUTH_USERNAME="xxxxxxxx" \
    -e BASIC_AUTH_PASSWORD="YYYYYYYY" \
```
    
Adjust alertmanager.yaml, example:
```
"global":
  "resolve_timeout": "5m"
"receivers":
  - name: 'telegram-webhook'
    webhook_configs:
    - url: http://telegram.telegram.svc.cluster.local:8080/alert
      send_resolved: true
      http_config:
        basic_auth:
          username: 'aiPh1eHu'
          password: 'eoPhait8'
"route":
  "group_by":
  - "job"
  "group_interval": "5m"
  "group_wait": "30s"
  "receiver": "telegram-webhook"
  "repeat_interval": "12h"
  "routes":
    - receiver: telegram-webhook
      match:
        alertname: Watchdog
    
```
