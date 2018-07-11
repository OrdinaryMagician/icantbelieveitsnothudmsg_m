# I Can't Believe It's Not HUDMessage

This is an example **API** for GZDoom's ZScript **HUDMessage** interface.

Hopefully it shouldn't be as ugly as **ACS** **HUDMessage**.

## The API

The main handler deals with queued messages every tick on play and ui sides.

Each **notHudMessage** class should have a companion **QueuedMsg** class. There
are two ways to create a message:

From play scope:
1. Call the static function `Create()` of the **notHudMessage** to get a
   **QueuedMsg**.
2. Call the static function `notHudMessageHandler.QueueMsg()` with the created
   message.

From ui scope:
1. Creation is the same as on play.
2. Call `AddSelf()` on the created message.

## Creating new messages

To implement new messages, you should create both a subclass of
**notHudMessage** and a subclass of **QueuedMsg**. Virtual functions to take
into account are:

* `notHudMessage.Setup()` : For initializing variables on creation. Parent
  class function must be called.
* `notHudMessage.DrawLine()` : Called for each line that has to be drawn.
* `notHudMessage.DoTick()` : Called each tic. Return true to mark the message
  as expired. Parent class function must be called.
* `QueuedMsg.AddSelf()` : For adding the message to the **Status Bar**.

In addition, each subclass of **notHudMessage** must implement its own static
clearscope `Create()` function, with its own set of parameters and return type,
to create an instance of its **QueuedMsg** subclass. Note that this is NOT a
virtual function, so there's no overrides to set or anything.

## Included message types

* **notHudMessage** : The standard, plain message. Will have fade in and fade
  out in the future.
* **notHudMessageTypeon** : (TODO) The text "types" itself character by
  character. Fade-in is handled per-character. Once the text is complete, it'll
  wait for the hold time then fade out as normal.
* **notHudMessageWaggle** : Example new effect. The text waggles in a wavy
  pattern.
