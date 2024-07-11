
## Overview

The `app.ts` file sets up the Express application, defining routes and middleware for the Caller Assistant application.

## Key Components

1. Express Application: Creates and configures the Express app.
2. Routes: Defines API endpoints for various functionalities.
3. Middleware: Sets up necessary middleware for the application.

## Main Routes

### GET /

- Purpose: Basic health check endpoint.
- Response: "Hello World 👋, from Caller Assistant!!"

### GET /calllog/:callsid

- Purpose: Retrieves call log for a specific call SID.
- Parameters: `callsid` (path parameter)
- Response: Call log data or error message.

### GET /applicationstatusjson/:callsid

- Purpose: Retrieves application status JSON for a terminated call.
- Parameters: `callsid` (path parameter)
- Response: Application status JSON or error message.

### POST /makeoutboundcall

- Purpose: Initiates an outbound call.
- Request Body: Call data including provider data and IVR menu.
- Response: Call initiation status and call SID.

### POST /callstatusupdate

- Purpose: Handles call status updates from Twilio.
- Request Body: Call status information.
- Response: 200 OK

### POST /hangupcall

- Purpose: Terminates an ongoing call.
- Request Body: `callSid`
- Response: 200 OK on success, 400 on failure.

## Middleware

```typescript
app.use(cors());
app.use(express.urlencoded({ extended: false }));
app.use(express.json());
app.use(express.static("dist"));

app.use(async (req, _, next) => {
  const protocol = "https";
  const wsProtocol = "wss";
  req.headers.protocol = protocol;
  req.headers.wsProtocol = wsProtocol;
  next();
});
```

- Enables CORS
- Parses URL-encoded bodies and JSON
- Serves static files from the "dist" directory
- Sets protocol headers for all requests

## Usage

Import and use in the main application file:

```typescript
import app from './app';
import { createServer } from 'http';

const server = createServer(app);
server.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```
