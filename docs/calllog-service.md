## Overview

`calllog-service.ts` provides functionality to manage call logs, including creating, reading, updating, and deleting log entries.

## Key Components

### CallLogKeys Enum

Defines keys for different types of call log data.

### CallLogService Object

Provides methods for interacting with call logs.

#### Methods

1. `create(sid: string, keyType: CallLogKeys, value: string | Record | CallApplicationJson | Array)`:

   - Purpose: Creates a new log entry.
   - Inputs: `sid` (call ID), `keyType` (log type), `value` (log data).

2. `read(sid: string)`:

   - Purpose: Retrieves the entire log for a specific call.
   - Input: `sid` (call ID).

3. `update(callSid: string, keyType: CallLogKeys.TRANSCRIPTION | CallLogKeys.IVR_TRANSCRIPTION, value: object)`:

   - Purpose: Updates transcription logs.
   - Inputs: `callSid`, `keyType`, `value` (new log entry).

4. `deleteCallLog(callSid: string)`:

   - Purpose: Deletes all log entries for a specific call.
   - Input: `callSid`.

5. `get(callSid: string, keyType: CallLogKeys)`:
   - Purpose: Retrieves a specific type of log entry for a call.
   - Inputs: `callSid`, `keyType`.

## Usage

```typescript
await CallLogService.create(callSid, CallLogKeys.CALL_STATUS, "in-progress");
const callLog = await CallLogService.read(callSid);
```
