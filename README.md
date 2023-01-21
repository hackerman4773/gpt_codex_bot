# gpt_codex_bot

Discord bot written in Python that uses the [completions API](https://beta.openai.com/docs/api-reference/completions) to have conversations with the `code-davinci-002` model, and the [moderations API](https://beta.openai.com/docs/api-reference/moderations) to filter the messages.

This bot uses the [OpenAI Python Library](https://github.com/openai/openai-python) and [discord.py](https://discordpy.readthedocs.io/).

This repo started as a fork of https://github.com/openai/gpt-discord-bot 

# Repo Outline:

- 'src' folder
    `base.py`: Contains the base class for the bot.
    `completion.py`: Handles the interaction with the OpenAI completions API to generate responses to user input.
    `constraints.py`: Handles constraints on the bot's behavior.
    `main.py`: Entry point for the bot, contains the main loop and initialization code.
    `moderation.py`: Handles the interaction with the OpenAI moderation API to filter messages.
    `utils.py:` Contains utility functions used throughout the code.
- `.env`: Contains environment variables used to configure the bot.
- `.gitignore`: Lists files and directories that should be ignored by git.
- `requirements.txt`: Lists the dependencies for the project, to be installed using pip.



# Features

- `/chat` starts a public thread, with a `message` argument which is the first user message passed to the bot
- The model will generate a reply for every user message in any threads started with `/chat`
- The entire thread will be passed to the model for each request, so the model will remember previous messages in the thread
- when the context limit is reached, or a max message count is reached in the thread, bot will close the thread
- you can customize the bot instructions through providing example conversations by modifying `config.yaml`

# Setup

1. - you can edit lines of 52-56 of `completion.py`
     ```engine="code-davinci-002", (OPEN AI MODEL)
            prompt=rendered,
            temperature=0.1,
            top_p=0.9,
            max_tokens=400,```
            

2. Copy `.env.example` to a new file called`.env` and start filling in the values as detailed below
    1. Go to https://beta.openai.com/account/api-keys, create a new API key, and fill in `OPENAI_API_KEY`
    2. Create your own Discord application at https://discord.com/developers/applications
        	1. Go to the Bot tab and click "Add Bot"
            - Click "Reset Token" and fill in `DISCORD_BOT_TOKEN`
            - Disable "Public Bot" unless you want your bot to be visible to everyone
            - Enable "Message Content Intent" under "Privileged Gateway Intents"
    3. Go to the OAuth2 tab, copy your "Client ID", and fill in `DISCORD_CLIENT_ID`
    4. Copy the ID the server you want to allow your bot to be used in by right clicking the server icon and clicking "Copy ID". (MUST HAVE DEV MODE TURNED ON IN YOUR DISCORD ACCOUNT)
    Fill in `ALLOWED_SERVER_IDS`. If you want to allow multiple servers, separate the IDs by "," like `server_id_1,server_id_2`

# Optional configuration

1. If you want moderation messages, create and copy the channel id for each server that you want the moderation messages to send to in `SERVER_TO_MODERATION_CHANNEL`. This should be of the format: `server_id:channel_id,server_id_2:channel_id_2`
2. If you want to change the personality of the bot, go to `src/config.yaml` and edit the instructions
3. If you want to change the moderation settings for which messages get flagged or blocked, edit the values in `src/constants.py`. A lower value means less chance of it triggering.


4. Install dependencies and run the bot
    Run the following command in your directory terminal:
    ```
    pip install -r requirements.txt
    python3 -m src.main
    ```
    You should see an invite URL in the console. Copy and paste it into your browser to add the bot to your server.

# FAQ

> Why isn't my bot responding to commands?

Ensure that the channels your bots have access to allow the bot to have these permissions.
- Send Messages
- Send Messages in Threads
- Create Public Threads
- Manage Messages (only for moderation to delete blocked messages)
- Manage Threads
- Read Message History
- Use Application Commands
