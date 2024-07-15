# Setting Up a Node.js Application on an EC2 Instance

This guide will walk you through the process of setting up a Node.js application on an Amazon EC2 instance. It includes installing Node.js, configuring environment variables, setting up a systemd service, and managing the application lifecycle.

## Step-by-Step Guide

### 1. Update and Upgrade the System

First, ensure your system is up-to-date by running the following commands:

```bash
sudo apt update
sudo apt upgrade
```

### 2. Install Node.js

Install Node.js using the NodeSource repository:

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
```

### 3. Create the Environment File

Create an environment file to store your application's environment variables:

```bash
sudo vim /etc/app.env
```

Add the following content to the file:

```
TWILIO_ACCOUNT_SID=
TWILIO_AUTH_TOKEN=
TWILIO_FROM_NUMBER=
TWILIO_TO_NUMBER=
PORT=
DEEPGRAM_API_KEY=
TWILIO_API_KEY=
TWILIO_API_SECRET=
OPEN_AI_KEY=
CALL_QUEUE_DELAY=
SERVER=
REDIS_CLIENT_URL=
WEBHOOK_URL=
```

Restrict the file permissions to ensure security:

```bash
sudo chmod 600 /etc/app.env
sudo chown ec2-user:ec2-user /etc/app.env
```

### 4. Create a systemd Service File

Create a systemd service file to manage your Node.js application:

```bash
sudo vim /etc/systemd/system/callerassistant.service
```

Add the following content to the file:

```
[Unit]
Description=Caller Assistant
After=network.target multi-user.target
[Service]
User=ec2-user
WorkingDirectory=/home/ec2-user/caller-assistant-prompt/backend
ExecStart=/usr/bin/node /home/ec2-user/caller-assistant-prompt/backend/build/index.js
Restart=always
Environment=NODE_ENV=production
EnvironmentFile=/etc/app.env
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=callerassistant
[Install]
WantedBy=multi-user.target
```

### 5. Reload systemd and Start Your Service

Reload the systemd daemon to recognize the new service and enable it to start on boot:

```bash
sudo systemctl daemon-reload
sudo systemctl enable callerassistant.service
sudo systemctl start callerassistant.service
```

Verify that the service is running properly:

```bash
sudo systemctl status callerassistant.service
```

### 6. View Logs

To view the logs for your service, use the following command:

```bash
sudo journalctl -fu callerassistant.service
```

## Understanding `package.json` Scripts

Here is an explanation of the scripts defined in your `package.json` file:

- **build**: Compiles TypeScript files and resolves module paths using `tsc` and `tsc-alias`.

  ```json
  "build": "tsc && tsc-alias"
  ```

- **start**: Builds the project and then starts the Node.js application, watching for changes.

  ```json
  "start": "npm run build && node ./build/index.js --watch"
  ```

- **dev**: Starts the application in development mode using `tsx` to watch for changes in `src/index.ts`.

  ```json
  "dev": "tsx watch src/index.ts"
  ```

- **lint**: Runs ESLint to check for code issues and Prettier to format the code.
  ```json
  "lint": "eslint . --ext .ts --fix  && prettier . --write"
  ```

For new changes, always rebuild and start the server by running:

```bash
npm run start
```

This setup ensures that your Node.js application is properly managed and can be easily maintained on an EC2 instance.

Caddy Setup:
[1] https://www.sammeechward.com/deploying-full-stack-js-to-aws-ec2
