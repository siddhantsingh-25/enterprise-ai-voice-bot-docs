# Call Status Meanings and Outbound Call Timeline

## Overview

The call status represents the current stage of a call's lifecycle. Different call status values indicate the progress of the call and provide insights into its state. This document explains the meaning of various call statuses and the typical timeline of an outbound call.

## Call Status Descriptions

### initiated

When the call status is `initiated`, it means that Twilio has removed the call from the queue and has started dialing the destination number.

### ringing

The `ringing` status indicates that the call has been successfully placed, and the recipient's device is ringing. This status signifies that the call is in the process of being answered.

### answered

If the call status is `answered`, it means that the recipient has answered the call. When this event is specified, Twilio will send an `in-progress` status, indicating that the call is actively in progress.

### completed

The `completed` status signifies that the call has ended, regardless of the reason for termination. The possible termination statuses associated with the `completed` status are:

#### Termination Statuses

- `busy`: The call was terminated because the recipient's line was busy.
- `cancelled`: The call was cancelled, either by the caller or the recipient.
- `completed`: The call was successfully completed, and both parties hung up normally.
- `failed`: The call failed due to an error or other issue.
- `no-answer`: The call was not answered by the recipient, and the call timed out or was terminated.
