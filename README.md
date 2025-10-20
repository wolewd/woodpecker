# woodpecker CI with Docker Compose

[![Docker](https://img.shields.io/badge/Docker-✔-2496ED?logo=docker&logoColor=white)](https://www.docker.com/) [![Caddy](https://img.shields.io/badge/woodpecker-✔-00CC88?logo=go&logoColor=white)](https://woodpecker-ci.org/)

This repository provides a simple and production-ready setup for **Woodpecker CI** as a CI/CD pipeline server.
It is designed for staging or production environments to simplify your deployment workflow.

---

## Purpose
Manually running `git push`, `git pull`, `docker down`, `build`, and `up` routines can be tedious. That's why I prepared this CI/CD setup to make my life easier.
I specifically chose an open-source CI/CD solution that's easy to set up *(sorry Jenkins)*, and I found that **Woodpecker** meets my needs perfectly.

## Prerequisites
- A domain
- A VPS or local machine running Linux (tested on Ubuntu & Arch Linux)
- Docker and Docker Compose installed
- A Docker network named application_network

---

## Installation & Usage

1. Clone this repository:
   ```bash
   git clone https://github.com/wolewd/woodpecker.git
   cd woodpecker
   ```

2. Create your `.env` file:
    ```bash
    touch .env
    ```

3. Configure your `.env` file
   Refer to the `.env.example` file for guidance. Here's a sample configuration:
    ```env
    # Port for the Woodpecker server
    WOODPECKER_SERVER_ADDR=:8001

    # Port for the agent to connect to the server
    WOODPECKER_GRPC_ADDR=:8002

    # Web UI address (replace with your domain)
    WOODPECKER_HOST=https://ci.your-domain.com

    # Shared secret between server and agent
    WOODPECKER_SECRET=supersecret

    # Git provider
    WOODPECKER_GITHUB=true

    # Your GitHub OAuth credentials
    WOODPECKER_GITHUB_CLIENT=your_github_client_id
    WOODPECKER_GITHUB_SECRET=your_github_client_secret

    # Set your github account to be admin
    WOODPECKER_ADMIN=your_github_username
    WOODPECKER_DEFAULT_TRUSTED=true

    # Agent connection target (Docker container format)
    WOODPECKER_SERVER=woodpecker-server:8002

    # Must match WOODPECKER_SECRET
    WOODPECKER_AGENT_SECRET=supersecret

    # Set to true to allow initial registration (disable after first login)
    WOODPECKER_OPEN=true

    # directory where you clone all of your repository to run in your vps [pwd]
    DIR_PATH=/home/user
    ```

4. Run the container:
    ```bash
    docker compose up -d
    ```

5. Access your site:
    ```bash
    https://ci-your-domain.com
    ```

6. Log in using your GitHub account, then shut down the containers:
    ```bash
    docker compose down
    ```

7. Update your `.env` file to disable open registration:
    ```env
    WOODPECKER_OPEN=false
    ```
    This prevents other users from registering new accounts.

---

## Getting Github client id and client secret

You can create new client inside your github account following this step:

1. Go to `Settings` -> `Developer Settings` -> `OAuth Apps` -> `New OAuth Apps`


2. Set `Application name` to anything you like


3. Set the `Homepage URL` to the domain you used in your `.env` file. *(e.g. https://ci-your-domain.com)*


4. Optionally, set the `Application description` to anything you like.


5. Set the `Authorization callback URL` to your domain followed by `/authorize`. *(e.g. https://ci-your-domain.com/authorize)*


6. Click `Register application`


7. Inside your new OAuth app, click `Generate a new client secret` and save it somewhere safe.

---

## Disclaimer

1. Make sure your domain or subdomain has proper DNS records set up.

2. This setup is intended for staging or production environments, and is not optimized for local development.

3. This repository does not yet include a guide for setting up CI/CD pipelines — I plan to add that soon.

4. You should create your own `.woodpecker.yaml` since I don't include the tutorial here. I just include `.woodpecker.yaml.example` for your references.

---
