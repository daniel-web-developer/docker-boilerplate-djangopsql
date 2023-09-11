# Boilerplate for development using Docker, Django, and Postgres

This is a simple boilerplate so I can start developing Django apps skipping the configuration part.

## Requirements:
You'll need to have [Python](https://wiki.python.org/moin/BeginnersGuide/Download) and [Docker](https://docs.docker.com/get-docker/) installed.

## Before running:

I recommend you to create another virtual environment on the project's root folder and install every dependency on /app/requirements.txt AFTER changing its versions. Keep reading to understand what I mean.

You'll probably want to change the following files:
- app/requirements.txt (this specifies the Django dependencies. Please pay attention to the packages versions, since you'll probably want to use current versions according to when you're running this);
- docker-compose.yml (change the version from postgres:${VERSION} to the one you wanna use);
- app/Dockerfile (change the Python image's version used. I still recommend using Alpine Linux (the -alpine at the end of the image's name specifies this) since it's really lightweight and the following commands in this file are meant specifically for its package manager. You might also wanna change the lines specifying the system dependencies (they start with `RUN apk update`). This might not be necessary and depends whether these containers, with the current configuration, will keep working in the future. Always check the logs in case of errors);
- .env.dev (you might wanna change this, but I don't recommend it. It's only the development environment and **YOU SHOULDN'T USE IT WHILE ON PRODUCTION ANYWAY**).

## Running:

**BEFORE ANYTHING: you might need to run each and every docker command as `root` with `sudo`.**

After cloning the repository, go to the root folder (the one that contains `docker-compose.yml`). These are the commands we're gonna use:
    - `docker compose up -d --build` to **build and run** the containers;
    - `docker compose down -v` to **shut down** the containers and the DB volume;
    - `docker compose logs -f` this might be the most important yet. If anything doesn't work, this will **show the logs** and tell you what's wrong;
    - `docker compose exec web python manage.py migrate --noinput` the `entrypoint.sh` file might not work for some reason and you might get the `The connection was reset` error. You'll need to run the command to run the migrations yourself.

**How to run the containers:**

1. Run `docker compose build` to build the containers;
2. Run `docker compose up -d` to run the containers;
3. If anything doesn't seem to work, check the logs with `docker compose logs -f`.
    - You might need to run `docker compose exec python manage.py migrate --noinput` yourself in case `entrypoint.sh` doesn't work.

**How to bring the containers down:**

- Run `docker compose down -v` to bring the contrainers and volumes down;
    - You might want to run `docker compose down -v --remove-orphans` the first time, but that's not exactly necessary. See if the terminal says anything about orphaned containers after inputting any of these commands.

**If anything doesn't work:**

- Check the logs with `docker compose logs -f`.

## If anything doesn't work:

Keep in mind this works perfectly in september of 2023. I literally just tested everything. However, assuming you're trying to use this 5 years from now, something might break. Check the logs for any errors.

