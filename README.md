# slack-OAuth-store
A wapper class of FileInstallationStore to support encryption at rest

## Example with Bolt
```
from slack_installation_store import EncryptedFileInstallationStore
from asyncio.log import logger
import os
import logging
from slack_bolt import App
from slack_bolt.oauth.oauth_settings import OAuthSettings
from slack_sdk.oauth.state_store import FileOAuthStateStore

logging.basicConfig(level=logging.DEBUG)

oauth_settings=OAuthSettings(
    client_id=os.environ.get("SLACK_CLIENT_ID"),
    client_secret=os.environ.get("SLACK_CLIENT_SECRET"),
    scopes=os.environ.get("SLACK_SCOPES","").split(','),
    user_scopes=[],
    install_path="/slack/install",\
    installation_store=EncryptedFileInstallationStore(base_dir="./data/installations", encryption_key=os.environ.get("SLACK_INSTALL_STORE_KEY")),
    state_store=FileOAuthStateStore(expiration_seconds=600, base_dir="./data/states")
)

# Initializes app with your bot oauth settings
app = App(
    signing_secret=os.environ["SLACK_SIGNING_SECRET"],
    oauth_settings=oauth_settings
)

@app.event("app_mention")
def event_test(body, say, logger):
    logger.info(body)
    say("Hello")

# Start your app
if __name__ == "__main__":
    app.start(port=int(os.environ.get("PORT", 3000)))

```

See https://github.com/slackapi/python-slack-sdk

