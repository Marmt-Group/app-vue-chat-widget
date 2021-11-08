<template>
<div>
  <div class="open-chat" :class="(isOpen ? 'hide' : 'show')" @click="toggleChatOpen">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1000 1000"><path :fill="iconColorProp" d="M899.3 171.4l-1.7 294.8s-4.9 159.7-176.2 159.7H481L373.9 736.3l176.2 1.6 242.1 250.3 8.3-256.9s189.4 13.2 189.4-154.8V327.8s8.2-100.3-90.6-156.4zM690.1 584.8c87.3 0 158.1-70.8 158.1-158.1V169.8c0-87.3-70.8-158.1-158.1-158.1h-522C80.8 11.7 10 82.5 10 169.8v256.9c0 87.3 70.8 158.1 158.1 158.1h13.3l1.6 233.9 261.2-233.9h245.9z"/></svg>
  </div>
  <div class="chat-container" :class="(isOpen ? 'show' : 'hide')">
    <div class="chat-window">
      <div class="close-chat" @click="toggleChatOpen" :style="{background: iconColorProp}">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 60 60"><g fill="none" stroke="#ffffff" stroke-width="10" stroke-miterlimit="10" stroke-linecap="round"><path d="M10 10l45 45M10 55l45-45"/></g></svg>
      </div>
      <div ref="chatArea" class="chat-area" :style="{background: messageBackgroundColorProp}">
        <p v-for="message in messageListProp" :key="message.id||message.body" class="message" :style="[message.author === 'you' ? {background: messageOutColorProp} : {background: messageInColorProp}]" :class="{'message-out': message.author === 'you', 'message-in': message.author !== 'you' }">
          {{ message.body }}
        </p>
      </div>
      <div class="chat-input">
        <form @submit.prevent="handleOutboundMessage()" class="chat-form">
          <input v-model="youMessage" type="text" :placeholder="messagePlaceholder" autofocus>
          <button class="submit" type="submit"><v-icon name="send" base-class="icon-send-message" :style="{color: iconColorProp, width: '30px'}"></v-icon></button>
        </form>
      </div>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: 'Chat',
  inheritAttrs: false,
  props: {
    messagePlaceholder: {
      type: String,
      default: 'Type your message'
    },
    iconColorProp: {
      type: String,
      default: '#e6e6e6'
    },
    messageBackgroundColorProp: {
      type: String,
      default: '#ffffff'
    },
    messageOutColorProp: {
      type: String,
      default: '#3d7e9a'
    },
    messageInColorProp: {
      type: String,
      default: '#f1f0f0'
    },
    messageListProp: Array,
    initOpenProp: Boolean
  },

  data: () => {
    return {
      youMessage: '',
      isOpen: false
    }
  },

  methods: {
    handleOutboundMessage() {
      if (!this.youMessage) {
        return
      }
      this.$emit('onMessageWasSent', { body: this.youMessage, author: 'you' })
      this.youMessage = ''
      this.$nextTick(() => {
        this.messageScroll()
      })
    },
    toggleChatOpen() {
      this.isOpen = !this.isOpen
      this.$emit('onToggleOpen', this.isOpen)
    },
    messageScroll() {
      let messageDisplay = this.$refs.chatArea
      messageDisplay.scrollTop = messageDisplay.scrollHeight
    }
  },
  mounted() {
    this.isOpen = this.initOpenProp || false
    if (this.messageListProp) {
      this.messageScroll()
    }
  }
}
</script>

<style scoped lang="css">
  * {
    box-sizing: border-box;
  }
  .headline {
    text-align: center;
    font-weight: 100;
    color: white;
  }
  .chat-container {
    position: fixed;
    bottom: 50px;
    right: 50px;
    max-width: 350px;
    width: 100%;
    z-index: 10000;
    transform: scale(0);
    transform-origin: right bottom;
  }
  .chat-container.show {
    animation: scaleIn .15s ease-in-out 0s 1 normal forwards;
  }
  .chat-container.hide {
    display: none;
  }
  .chat-window {
    display: flex;
    flex-direction: column;
    box-shadow: 2px 2px 10px 2px rgba(0, 0, 0, 0.1);
    width: 100%;
  }
  .chat-area {
    border-radius: 3px 3px 0 0;
    height: 300px;
    padding: 1em 1em 0;
    position: relative;
    overflow: auto;
    width: 100%;
  }
  .message {
    width: 45%;
    border-radius: 10px;
    padding: .5em;
    font-size: .8em;
  }
  .message-out {
    color: #ffffff;
    margin-left: 50%;
  }
  .message-in {
    background: #f1f0f0;
    color: black;
  }
  .chat-inputs {
    display: flex;
    justify-content: space-between;
  }
  .chat-input input {
    border: none;
    font-size: .8em;
    outline: none;
    padding: 1.5em;
    width: 90%;
  }
  .chat-form {
    background: #ffffff;
    border-top: 1px solid #e9e9e9;
    border-radius: 0 0 3px 3px;
    display: flex;
    flex-direction: row;
    align-items: center;
    overflow: hidden;
    width: 100%;
  }
  .submit {
    -webkit-appearance: none;
    background: transparent;
    border: 0;
    cursor: pointer;
  }
  .submit:focus {
    outline: none;
  }
  .submit-icon {
    width: 20px;
  }
  .close-chat {
    position: absolute;
    right: -40px;
    top: -40px;
    width: 35px;
    border-radius: 50%;
    height: 35px;
    background: #f7f7f7;
    cursor: pointer;
    transform: scale(0);
  }
  .chat-container.show .close-chat {
    animation: scaleIn .15s ease-in-out .3s 1 normal forwards;
  }
  .close-chat svg {
    position: relative;
    left: -1px;
    top: 6px;
    width: 20px;
  }
  .open-chat {
    position: fixed;
    width: 70px;
    right: 50px;
    bottom: 25px;
    cursor: pointer;
    z-index: 900;
    transform: scale(0);
  }
  .open-chat.hide {
    display: none;
  }
  .open-chat.show {
    animation: scaleIn .15s ease-in-out .15s 1 normal forwards;
  }
  @keyframes scaleIn {
    0% {
      transform: scale(0);
    }
    100% {
      transform: scale(1);
    }
  }
</style>
