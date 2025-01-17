# Insurance Starter Pack

## Background

This starter pack is to help get started on insurance related tasks. The starter pack demonstrates a few different areas
where a Rasa assistant can help with insurance-related functions, getting information for an insurance quote, handling
common tasks like ordering a new ID card, and helping customers manage their claims.

## Run the Bot

Before training your bot make sure you have installed all of the requirements from the `requirements.txt` file:

```bash
pip3 install -r requirements.txt
```

To run the bot locally you must first train your bot by opening a terminal window in the project directory. In the terminal
window enter:

```bash
rasa train
```

This will train a model for your bot that you'll be able to interact with in the next steps. You can find out more about
[rasa train](https://rasa.com/docs/rasa/command-line-interface/#rasa-train) in the Rasa docs!

When you're training successfully completes you will have a model artifact. Before you can start interacting with your
bot the [Action Server](https://rasa.com/docs/action-server/) also needs to be running. The action server handles custom 
processing of messages. Starting the action server requires opening a new terminal window in your project directory. In
the window enter:

```bash
rasa run actions
```

You will see a listing of the different actions that are a part of the server. You will need to keep this terminal window
open.

Finally, the last piece is to start a Duckling server. The [Duckling server](https://rasa.com/docs/rasa/components/#ducklinghttpextractor) will
help the bot robustly extract numbers from the user messages. Open one more terminal window in your project root and enter:

```bash
docker run -p 8000:8000 rasa/duckling
```

Similar to the Action Server keep this running while you interact with your bot.

Now you can talk with the bot! In a terminal window enter:

```bash
rasa shell
```

This command will allow you to talk with the bot. If you want more more detail about what's happening with your bot you
can add `--debug` to the command to display all of the debugging information.

## What the Bot Does

Right now the bot accomplishes these core insurance functions:

1. Get a Quote
2. Order a New ID Card
3. Check Claim Status
4. Pay Outstanding Claim Balance
5. File a New Claim

The demo currently has mock data for a customer with a handful of claims to scroll through to demonstrate 

## Rasa X Deployment

The bot is ready to be deployed to a Rasa X instance. The easiest way for you to deploy your bot to Rasa X is to utilize
the [one line deployment script](https://rasa.com/docs/rasa-x/installation-and-setup/install/quick-install-script/).

Once Rasa X is running you can use the [git integration](https://rasa.com/docs/rasa-x/installation-and-setup/deploy/#integrated-version-control) to
load your bot into the Rasa X instance.

### Create the Action Server

You will need to have docker installed in order to build the action server image. If you haven't made any changes to the action code, you can also use the [public image on Dockerhub](https://hub.docker.com/repository/docker/mvielkind/insurance_pack) instead of building it yourself.

It is recommended to use an [automated CI/CD process](https://rasa.com/docs/rasa/user-guide/setting-up-ci-cd) to keep your action server up to date in a production environment.


## Run Bot server using Docker compose on local machine and connect it with telegram

This repository contains 2 docker-compose files which are going to help you in trainning and inference process.

First make sure you have docker installd on pc, you can download it from the [official website](https://www.docker.com/products/docker-desktop)


### Trainning
This first step is really importan. On root folder make a copy of the file .env.example and rename it .env 

To train the bot. In the terminal window enter:

```bash
docker-compose -f docker-compose-trainning.yml up
```

When trainning finishes you will seee a success message on console, so you can stop containers using with this command:

```bash
docker-compose down --remove-orphans
```

As result you will have a new tranning model will b available on models folder

### Connecting Telegram

If you are running bot on a local pc please creat an account and install [ngrok](https://ngrok.com/product) on your pc. This tool is going to provied you with a public IP address and https security. 
After that run on a command line:

```bash
./ngrok http 5005
```

On .env file edit TELEGRAM_ACCESS_TOKEN, TELEGRAM_VERIFY, you can find how to get this variables using telegram's Bot Father [here ](https://rasa.com/docs/rasa/connectors/telegram/)

Add your engok https url on TELEGRAM_WEBHOOK_URL variable

### Start the bot
To run the bot just enter in a command line:

```bash
docker-compose up
```

You can check the og if anything goes wrong on logs folder

Finally you can talk with the bot using telegram, just search bot your bot and start writting. When you want to stop the bot just run.

```bash
docker-compose down --remove-orphans
```