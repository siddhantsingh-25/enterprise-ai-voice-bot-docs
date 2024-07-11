

## Overview

`activecall-service.ts` implements the `ActiveCallConfig` class, which manages the configuration and state of active calls in the application.

## Key Components

### ActiveCallConfig Class

This class follows the Singleton pattern to ensure only one instance of call configuration exists throughout the application lifecycle.

#### Properties

- `callConfig`: Stores the current call configuration.

#### Methods

1. `getInstance()`: Static method to get or create the singleton instance.

2. `setCallConfig({ callSid, ivrMenu, providerData })`: 
   - Purpose: Sets up a new call configuration.
   - Inputs: `callSid` (string), `ivrMenu` (Array), `providerData` (Record).

3. `setIVRNavigationCompleted()`:
   - Purpose: Marks the IVR navigation as completed for the current call.

4. `setIsLastIVRMenuOptionUsed()`:
   - Purpose: Marks that the last IVR menu option has been used.

5. `getCallConfig()`:
   - Purpose: Retrieves the current call configuration.

6. `deleteCallConfig()`:
   - Purpose: Clears the current call configuration.

7. `resetCallConfig()`:
   - Purpose: Resets the call configuration while maintaining the IVR navigation completion status.

## Usage

```typescript
const activeCallConfig = ActiveCallConfig.getInstance();
activeCallConfig.setCallConfig({
  callSid: 'CA123456',
  ivrMenu: [...],
  providerData: {...}
});
```
