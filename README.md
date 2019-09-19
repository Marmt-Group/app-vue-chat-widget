# vue-chat-widget
`vue-chat-widget` is a simple chat window that can be included easily in any Vue project. It is backend agnostic and as such provides no messaging facilities, only the UI component.
<br/>

![Demo gif of react-chat-window being used](https://media.giphy.com/media/XEsOASAkabaoCm1chn/source.gif)

## Features

- Customizeable
- Backend agnostic
- No file input's or emojis
- Free

## [Demo](https://vue-chat-window-david-j-davis.marmt-group.now.sh/)

## Table of Contents
- [Installation](#installation)
- [Example](#example)
- [Components](#components)
- [Backend Example with WebSockets](#backend)

## Installation

```
$ npm install vue-chat-widget
```

## Example

``` javascript
<template>
    <div id="app">
      <Chat
        iconColorProp="#e6e6e6"
        messageOutColorProp="#4d9e93"
        messageInColorProp="#f1f0f0"
        messageBackgroundColorProp="#ffffff"
        :messageListProp="messageList"
        :initOpenProp="initOpen"
        @onToggleOpen="handleToggleOpen"
        @onMessageWasSent="handleMessageReceived"
      />
    </div>
</template>

<script>
import {Chat} from 'vue-chat-widget'
import incomingMessageSound from '../assets/notification.mp3' // pick an audio file for chat response

export default {
  name: "app",
  components: {
    Chat,
  },
  data: () => {
    return {
      messageList: [],
      initOpen: false,
      toggledOpen: false
    }
  },
  methods: {
    // Send message from you
    handleMessageReceived(message) {
      this.messageList.push(message)
    },
    // Receive message from them (handled by you with your backend)
    handleMessageResponse(message) {
       if (message.length > 0) {
            this.messageList.push({ body: message, author: 'them' })
        }
    },
    // Chat toggled open event emitted
    handleToggleOpen(open) {
      this.toggledOpen = open
      // connect/disconnect websocket or something
    },
    // Audible chat response noise, use whatever noise you want
    handleMessageResponseSound() {
      const audio = new Audio(incomingMessageSound)
      audio.addEventListener('loadeddata', () => {
        audio.play()
      })
    },
  },
  // init chat with a message
  mounted() {
    this.messageList.push({ body: 'Welcome to the chat, I\'m David!', author: 'them' })
  },
  watch: {
    messageList: function(newList) {
      const nextMessage = newList[newList.length - 1]
      const isIncoming = (nextMessage || {}).author !== 'you'
      if (isIncoming && this.toggledOpen) {
        this.handleMessageResponseSound()
      }
    }
  }
}
</script>
```

## Notification Sound

Implement your own notification mp3 sound “🔔”, or [download this mp3 file](https://www.dropbox.com/s/fnnjo8cyxj8ovgf/notification.mp3?dl=1).

## Components

## Chat

`Chat` is the only component needed to use vue-chat-widget. It will react dynamically to changes in messages. All new messages must be added via a change in props as shown in the example.

Launcher props:

|      prop        | type   | required | description |
|------------------|--------|----------|-------------|
| iconColorProp     | String | no | Set icon color for close and open icons. Defaults to `#e6e6e6`|
| messageOutColorProp | String | no | Set color of outgoing messages. Defaults to `#3d7e9a` |
| messageInColorProp | String | no | Set color of incoming messages. Defaults to `#f1f0f0` |
| messageBackgroundColorProp | String | no | Set background color of message area. Default to `#ffffff` |
| initOpenProp | Boolean | yes | Force the open/close state of the chat window on mount. |
| messageListProp  | Array [[message](#message-objects)] | yes | An array of message objects to be rendered as a conversation. |
| @onToggleOpen    | event | yes | Event emitted when chat window is open and closed. |
| @onMessageWasSent | function([message](#message-objects)) | yes | Emitted when a message is sent, with a message object as an argument. |


### Message Objects

Message objects are rendered differently depending on their type. Currently, only text types are supported. Each message object has an `author` field which can have the value 'you' or 'them'.

``` javascript
{
  author: 'them',
  body: 'some text'
}

{
  author: 'you',
  body: 'some text'
}

```

## Backend

For an example frontend using vue-chat-widget and websockets, [look here](https://gist.github.com/david-j-davis/afcd4bcceaa7562f91604b945dd3f8b0). For an accompanying backend in Node.js for this, [look here](https://gist.github.com/david-j-davis/5f88958874a0acdf0a9bde30ddfc1a23).