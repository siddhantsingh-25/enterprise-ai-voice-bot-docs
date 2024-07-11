## API Documentation

**Base URL:** `/`

### Endpoints

#### GET /

- **Description:** Returns a simple greeting message.

- **Response:**

  - Status code: 200
  - Body: `"Hello World 👋, from Caller Assistant!!"`

#### GET /calllog/:callsid

- **Description:** Fetches the call log details for a given call SID.

- **Parameters:**

  - `callsid` (string, required): The unique identifier for the call.

- **Example Request:**

```
GET /calllog/CAxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

- **Responses:**

  - Status code: 200 (OK)
  - Body:
    ```json
    {
      "sid": "CAxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
      "callstatus": "completed",
      "transcription": [
        { "role": "user", "content": "Hello" },
        { "role": "assistant", "content": "Welcome to Caller Assistant! How can I help you today?" }
      ],
      "ivr--transcription": [
        { "role": "user", "content": "1" }, // Pressed 1 on IVR
        { "role": "assistant", "content": "You selected option 1. Transferring you now..." }
      ],
      "updatedAt": 1682345678,
      "applicationStatus": { "status": "submitted", "data": [...] }
    }
    ```
  - Status code: 400 (Bad Request)

    - Body: `{ message: "call sid is missing or invalid" }`

  - Status code: 404 (Not Found)
    - Body: `{ message: "Call Details not found for CallSid: CAxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx." }`

#### GET /applicationstatusjson/:callsid

- **Description:** Retrieves the application status JSON for a given call SID, only if the call has finished.

- **Parameters:**

  - `callsid` (string, required): The unique identifier for the call.

- **Example Request:**

```
GET /applicationstatusjson/CAxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

- **Responses:**

  - Status code: 200 (OK)
    - Body:
    ```json
    {
      "status": "enrollmentinprocess",
      "data": [
        {
          "key": "applicationacknowledged",
          "value": "yes"
        },
        {
          "key": "deficiencyreported",
          "value": "no"
        },
        {
          "key": "applicationtrac kingnumber",
          "value": "N/A"
        },
        {
          "key": "nextfollowupdate",
          "value": "02122024"
        },
        {
          "key": "notes",
          "value": "demotext"
        }
      ]
    }
    ```
  - Status code: 400 (Bad Request)
    - Body:
      - `{ message: "call sid is missing or invalid" }`
      - `{ message: "Call Has not finished yet, cannot get Application Status Json." }`
      - `{ message: "Unable to find Transcription." }`

#### POST /makeoutboundcall

- **Description:** Initiates an outbound call.

- **Request Body:**

```typescript
{
  ivrMenu: {
    intent: string;
    triggers: string[];
    response: string;
  }[];
  providerData: {
    phoneNumber: string;
    // ... other provider-specific data
  };
}
```

- **Example Request:**

```json
{
  "ivrMenu": [
    {
      "intent": "Enter NPI",
      "response": "{providerNpi}",
      "triggers": ["enter npi"]
    },
    {
      "intent": "Verify NPI",
      "response": "1",
      "triggers": ["{providerNpi} correct"]
    },
    {
      "intent": "Reason for your Call",
      "response": "dental provider",
      "triggers": ["dental provider"]
    },
    {
      "intent": "Confirm Reason for Call",
      "response": "credentialing",
      "triggers": ["credentialing"]
    }
  ],
  "providerData": {
    "ticketId": 170980,
    "ticketName": "Application Follow-up",
    "taskType": "Application followup by Phone",
    "payerName": "United Healthcare Insurance Company (Pennsylvania)",
    "groupTaxId": "991837914",
    "groupName": "VENDELA COUNSELING LLC",
    "groupNpi": "1194583237",
    "groupSpeciality": "Adult Mental Health Clinic/Center",
    "providerName": "KATLYN JANE LICHTY",
    "providerNpi": 1215673181,
    "providerSpeciality": "Professional Counselor",
    "serviceState": "PA",
    "locationAddress": "230 HARRISBURG AVE STE 2  LANCASTER PA 17603",
    "specialityType": "Behavioral Health Providers",
    "phoneNumber": "8776140484",
    "applicationSubmitionDate": "04/19/2024 12:00:00 AM EST",
    "applicationTrackingNumber": "",
    "callbackNumber": "8264549832"
  }
}
```

- **Responses:**

  - Status code: 200 (OK)
    - Body:

```json
{
  "message": "Call initiated",
  "callSid": "CAf92dbd586945880050d124cf0b52d2ae",
  "updatedIvrMenu": [
    {
      "intent": "Enter NPI",
      "response": "1215673181",
      "triggers": ["enter npi"]
    },
    {
      "intent": "Verify NPI",
      "response": "1",
      "triggers": ["1215673181 correct"]
    },
    {
      "intent": "Reason for your Call",
      "response": "dental provider",
      "triggers": ["dental provider"]
    },
    {
      "intent": "Confirm Reason for Call",
      "response": "credentialing",
      "triggers": ["credentialing"]
    }
  ]
}
```

- Status code: 400 (Bad Request)
  - Body:
    - If a call is in progress or the request is malformed, the following responses are possible:

```json
{
  "message": "Cannot initiate another call when a previous call is in progress.",
  "callSid": null
}
```

```json
{
  "message": "IVR Menu is missing.",
  "callSid": null
}
```

```json
{
  "message": "Invalid Provider Data, PhoneNumber is Missing.",
  "callSid": null
}
```

> **Note:** The `ivrMenu` can contain keys with the syntax `{providerDataKey}`. These keys will be replaced with the corresponding values from the `providerData` object at runtime. For example, `{providerNpi}` will be replaced with the actual NPI value from the `providerData`.

#### POST /callstatusupdate

- **Description:** Updates the status of a call.

- **Request Body:**

  - `CallSid` (string): The unique identifier for the call.
  - `CallStatus` (string): The new status for the call.

- **Example Request:**

```json
{
  "CallSid": "CAxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
  "CallStatus": "in-progress"
}
```

- **Response:**

  - Status code: 200 (OK)

#### POST /hangupcall

- **Description:** Hangs up a call.

- **Request Body:**

  - `callSid` (string): The unique identifier for the call.
  - **Example Request:**

```json
{
  "callSid": "CAxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
}
```

- **Responses:**

  - Status code: 200 (Success)
  - Status code: 400 (Failure)
