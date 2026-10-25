# Phantom Fob — TryHackMe

Room: https://tryhackme.com/room/phantomfob

The fob can lock the demo car and sound its horn, but it has no unlock button. The room gives access to a simulated vehicle's CAN bus and asks me to understand the messages well enough to construct the missing unlock command. The important idea is that the fob is sending messages on a shared vehicle network, not communicating through magic.

## Understand the CAN bus

A vehicle's Controller Area Network lets electronic control units exchange small messages. A CAN frame includes an identifier and data bytes. The identifier helps describe the message's purpose or priority, while the data carries the payload. The exact interpretation depends on the vehicle's message definitions.

The room uses a simulated environment, so I should start the provided lab machine and follow its instructions for connecting to the CAN interface. I should not connect these experiments to a real vehicle.

## Observe before sending

The sensible workflow is to capture the known lock and horn actions, compare the frames, and identify which parts change between actions. If the room's environment provides tools such as `candump` or a socketcand connection, I use the exact interface name and command shown by the lab. I record the CAN identifier and data bytes for each known action before trying to infer the unlock message.

A replayed frame may fail if the data changes over time or includes a rolling value or checksum. That is why the task warns that codes change every second. I need to work out the message structure and timing from the observed lab traffic rather than assume the same bytes will always work.

## Craft and validate the unlock message

Once I understand the format, I follow the room's instructions to construct the unlock frame in the simulated environment and verify that the car responds as expected. I should change one part at a time and preserve the evidence that supports my interpretation. I should not publish a guessed frame as a confirmed result.

## What I took from this room

The room demonstrates why observing a wireless key fob alone is not the whole problem: messages on the vehicle network may contain structure, timing, and changing values. A secure design needs authentication and replay protection, not just a message format that is difficult to guess. The final flag should be replaced with `flag{hidden}` in the public version after I verify it in the active room.