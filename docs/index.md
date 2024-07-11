# Documentation for index.ts

## Overview

`index.ts` is the main entry point of the Caller Assistant application. It sets up the server, WebSocket connections, and integrates various services to handle real-time communication for the voice assistant.

## Key Components

1. HTTP Server
2. WebSocket Server
3. Service Integrations:
   - StreamService
   - TranscriptionService
   - GPTService
   - IVRService
   - ActiveCallConfig

## Functionality

### Server Setup

- Creates an HTTP server using the Express app.
- Initializes a WebSocket server on top of the HTTP server.

### WebSocket Connection Handling

- Manages new client connections.
- Sets up event listeners for WebSocket messages.

### Service Integration

- Initializes and coordinates between various services:
  - StreamService: Handles media streaming.
  - TranscriptionService: Manages speech-to-text conversion.
  - GPTService: Processes natural language and generates responses.
  - IVRService: Manages Interactive Voice Response menus.

### Event-Driven Architecture

- Utilizes event listeners to handle asynchronous operations between services.

## Key Functions

### startServer()

- Initializes the server and sets up WebSocket connections.
- Handles incoming WebSocket messages and routes them to appropriate services.

### WebSocket Event Handlers

- `"message"`: Processes incoming Twilio messages (start, media, stop events).
- `"close"`: Handles client disconnection.
- `"error"`: Manages WebSocket errors.

### Service Event Handlers

- Transcription events: Processes transcribed text and routes to GPT or IVR services.
- GPT events: Handles GPT responses and sends them to the stream service.
- IVR events: Manages IVR responses and sends them to the stream service.

## Usage

The file is executed to start the server:

```bash
npm run start
```

## Dependencies

- `http`: Node.js HTTP module
- `ws`: WebSocket library
- `@scripts/iscall-transfered`: Custom script for call transfer logic
- `@service/*`: Various custom services (activecall, gpt, ivr, redis, stream, transcription)
- `@utils/*`: Utility functions and configurations

