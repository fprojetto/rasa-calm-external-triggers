# Rasa CALM external triggers

The goal of this project is to showcase a flow that can be triggered from an external system using the Rasa endpoint: `/conversations/<conversation_id>/trigger_intent`.

## Project Structure

<table border="1">
   <tr>
   <th>Flow Name</th>
   <th>Description</th>
   <th>Link to flow</th>
   <th>Link to domain</th>
   </tr>

   <tr>
      <td>Tell Conversation ID</td>
      <td>Gives you the conversation ID.</td>
      <td><a href="data/flows/hello.yml">Link</a></td>
      <td><a href="domain/hello.yml">Link</a></td>
   </tr>
   
   <tr>
      <td>External Greeting</td>
      <td>Flow that should be triggered using an intent activated from the Rasa API.</td>
      <td><a href="data/flows/hello.yml">Link</a></td>
      <td><a href="domain/hello.yml">Link</a></td>
   </tr>
</table>

## How to run this project

### Configuration

This project uses [cohere](https://cohere.com/) as LLM mainly because you can run this project free of costs.

1. [Install Rasa Pro](https://rasa.com/docs/rasa-pro/installation/python/installation).

2. Setup credentials.

```
export RASA_PRO_LICENSE=<YOUR_RASA_LICENSE>
export COHERE_API_KEY=<YOUR_COHERE_API_KEY>
```

3. Create a model using.

 `rasa train`

### Run the components

1. Run the action server in a different tab.

`rasa run actions`

2. Run the Rasa server with an interactive shell.

`rasa shell --enable-api`
 
### Run the manual test case to showcase how external triggers work in Rasa CALM

1.  Get the conversation ID.

From the Rasa shell, get the conversation ID typing something like: `I'd like to have the conversation ID`. It uses a [custom action](actions/action_tell_id.py) to get it.

2. Trigger the intent using the Rasa API.
 - Set the conversation ID.
 
 `export conversation_id=<conversation_id>`
 - Trigger the intent.

    ```
    curl -H "Content-Type: application/json" -X POST -d \
    '{"name": "EXTERNAL_hello"}' \
    "http://localhost:5005/conversations/${conversation_id}/trigger_intent"
    ```

3. Verufy that the response body containins a json with the `messages` field set to `["Hi, dude! This message has been triggered by an external system, yeah!"]`
