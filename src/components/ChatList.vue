<template>
  <div class="communication">
    <h2 class="communication__head">
      Комната {{ this.$store.state.roomCurrent }}
    </h2>
    <ul class="chat-list">
      <li class="chat-list__el" v-for="msg in chat" v-bind:key="msg.index">
        <span class="chat-list__msg">{{ msg.text }}</span>
        <span class="chat-list__person"> {{ msg.id}} </span>
        <span class="chat-list__date">{{ msg.created }}</span>
      </li>
    </ul>
    <NewMsg :isChat="true" @submit-new-msg="onSubmit"></NewMsg>
  </div>
</template>

<script>
  import NewMsg from "@/components/NewMsg";
  export default {
    name: "ChatList",
    components: {NewMsg},
    props: {
      "chat" : Array
    },
    methods: {
      onSubmit: function(){
        this.$emit("submit-new-msg")
      }
    },
    updated() {
      let chatList = document.getElementsByClassName("chat-list")[0]
      chatList.scrollTop = chatList.scrollHeight
    }
  }
</script>

<style>
  .communication {
    display: grid;
    grid-template-columns: 840px 200px;
    grid-template-rows: 50px 330px 100px;
    grid-row: 2 / 4;
    grid-column: 2 / 4;

  }

  .communication__head {
    grid-row: 1;
    grid-column: 1;
  }

  .chat-list {
    margin-bottom: 15px;

    grid-row: 2;
    grid-column: 1;

    overflow: hidden;
    overflow-y: scroll;
  }

  .chat-list__el {
    width: 750px;

    display: flex;
    flex-wrap: wrap;
    position: relative;

    margin-bottom: 15px;
    padding: 5px 25px 0 25px;

    background-color: #EFF1A8;
    border-radius: 30px;
    border-bottom: 1px solid gainsboro;
  }

  .chat-list__msg {
    width: inherit;
    padding-bottom: 15px;

    overflow-wrap: break-word;

  }

  .chat-list__person, .chat-list__date {
    position: absolute;
    bottom: -12px;
  }

  .chat-list__person {
    margin-right: auto;
    left: 20px;
  }

  .chat-list__date {
    right: 20px;
  }

  .communication .msg-new {
    grid-row: 3;
    grid-column: 1 / 3;

    top: 0;
  }
</style>