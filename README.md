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

## Installation

```
$ npm install vue-chat-widget
```

## Example

``` javascript

```

## Components

## Launcher

`Launcher` is the only component needed to use vue-chat-widget. It will react dynamically to changes in messages. All new messages must be added via a change in props as shown in the example.

Launcher props:

 iconColorProp="#e6e6e6"
        messageOutColorProp="#4d9e93"
        messageInColorProp="#f1f0f0"

|      prop        | type   | required | description |
|------------------|--------|----------|-------------|
| iconColorProp     | String | no | Set icon color for close and open icons. Defaults to `#e6e6e6`|
| messageOutColorProp | String | no | Set color of outgoing messages. Defaults to `#3d7e9a` |
| messageInColorProp | String | no | Set color of incoming messages. Defaults to `#f1f0f0` |
| initOpenProp | Boolean | yes | Force the open/close state of the chat window on mount. |
| messageListProp  | Array [[message](#message-objects)] | yes | An array of message objects to be rendered as a conversation. |
| muteSoundProp | Boolean | no | Don't play sound for incoming messages. Defaults to `false`. |
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