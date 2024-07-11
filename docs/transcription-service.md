## Overview

`transcription-service.ts` implements the `TranscriptionService` class, which handles real-time speech-to-text transcription using the Deepgram API.

## Key Components

### TranscriptionService Class

Manages the connection to Deepgram and processes incoming audio data for transcription.

#### Properties

- `deepgramLive`: Instance of Deepgram's LiveClient.
- `audioBuffer`: Buffer to store audio data.
- `finalResult`: Stores the final transcription result.
- `speechFinal`: Boolean flag indicating if speech is final.

#### Methods

1. `constructor()`:
   - Initializes the Deepgram client and sets up event listeners.

2. `send(payload)`:
   - Purpose: Sends audio data to Deepgram for transcription.
   - Input: `payload` (audio data).

3. `sendBufferedData(bufferedData)`:
   - Purpose: Sends buffered audio data to Deepgram with retry logic.
   - Input: `bufferedData` (Buffer of audio data).

#### Event Handlers

- Handles various Deepgram events like 'Open', 'Close', 'Transcript', 'UtteranceEnd', and 'Error'.

## Usage

```typescript
const transcriptionService = new TranscriptionService();
transcriptionService.on('transcription', (text) => {
  console.log('Transcribed text:', text);
});
transcriptionService.send(audioPayload);
```
