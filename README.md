# External Triggers

The goal of this project is to showcase a flow that can be triggered from an external system using the Rasa endpoint: `/conversations/<conversation_id>/trigger_intent`.

## How to run this project

1. Setup credentials
    ```
    RASA_PRO_LICENSE=<YOUR_RASA_LICENSE>
    COHERE_API_KEY=<YOUR_COHERE_API_KEY>
    ```

2.  Setup python environment and install Rasa
    ```
    python3.10 -m venv .venv

    cat > .venv/pip.conf <<-EOF
    [global]
    extra-index-url = https://europe-west3-python.pkg.dev/rasa-releases/rasa-plus-py/simple/
    EOF

    source .venv/bin/activate

    pip install rasa-pro
    ```

3. Create a model using `rasa train`

4. Run the action server in a different tab: `rasa run actions`

5. Run the Rasa server with an interactive shell and get the conversation ID
 - `rasa shell --enable-api`
 - From the shell, get the conversation ID typing something like: `I'd like to have the conversation ID`. It uses a [custom action](actions/action_tell_id.py) to get it.

6. Trigger the intent using the Rasa API:
 - set the conversation id: `conversation_id=<conversation_id>`
 - Trigger the intent:
    ```
    curl -H "Content-Type: application/json" -X POST -d \
    '{"name": "EXTERNAL_hello"}' \
    "http://localhost:5005/conversations/${conversation_id}/trigger_intent"
    ```

7. Expected a response containing a json with the `messages` field set to `["Hi, dude! This message has been triggered by an external system, yeah!"]`
