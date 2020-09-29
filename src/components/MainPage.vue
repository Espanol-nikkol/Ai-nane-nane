<template>
  <div class="chat">
    <h1 class="chat__head">
      Ай-нане-нане, добро пожаловать в чат, {{this.$store.state.user}}
    </h1>
    <ListRoom :rooms=rooms v-if="rooms" @room-change="onRoomChange" @open-new-room="onNewRoomClick"></ListRoom>
    <NewMsg :isChat="false" v-if="showNewRoom" @submit-new-msg="sendMsg"></NewMsg>
    <Authorization :show-auth="showAuth" @btn-auth-click="toggleVisibleAuth" @submit-auth="auth"></Authorization>
    <chat-list v-if="showChat" :chat="chat" @submit-new-msg="sendMsg"></chat-list>
  </div>
</template>

<script>
import ListRoom from "@/components/ListRoom";
import NewMsg from "@/components/NewMsg";
import Authorization from "@/components/Authorization";
import ChatList from "@/components/ChatList";

const ROOMS = "https://nane.tada.team/api/rooms"
const TIME_OPTIONS = {
  hour: 'numeric',
  minute: 'numeric',
  year: 'numeric',
  month: 'numeric',
  day: 'numeric'
}

export default {
  name: "MainPage",
  components: {ChatList, Authorization, NewMsg, ListRoom},
  data() {
    return {
      rooms: null,
      showNewRoom: false,
      server: null,
      showAuth: false,
      showChat: false,
      chat: []
    }
  },
  methods: {
    onNewRoomClick: function() {
      if (this.showAuth) this.showAuth = false
      if (this.showChat) this.showChat = false
      this.showNewRoom = !this.showNewRoom
    },
    toggleVisibleAuth: function() {
      this.showAuth = !this.showAuth
    },
    getRooms: function() {
      fetch(ROOMS).then(ans => ans.json().then(json => this.rooms = json.result.sort()))
    },
    getId: function() {
      let id = "";
      let user = this.$store.state.user
      let id_input = document.getElementById("user")
      console.log(id_input)
      if (id_input !== null && id_input.value !== "") {
        id = user + "(" + id_input.value + ")"
      } else {
        id = user
      }
      return id;
    },
    getRecord: record => {
      record.created = new Date(record.created).toLocaleString('ru-RU',TIME_OPTIONS)
      return record
    },
    getHistory: function (nameRoom) {
      fetch("https://nane.tada.team/api/rooms/" + nameRoom + "/history").
      then(resp => resp.json().
        then(json => {
          this.chat = json.result.map(i => this.getRecord(i))
        })
      )
    },
    sendMsg: function() {
      let room = document.getElementById("room-name")
      room = room ? room.value : this.$store.state.roomCurrent
      let text_input = document.getElementById("text-msg")
      let data = {
        "room": room,
        "text": text_input.value.trim(),
        "id": this.getId()
      }
      this.server.send(JSON.stringify(data))
      text_input.value = ""
      if (this.showNewRoom) this.showNewRoom = false
      if (!this.rooms.includes(room)) {
        this.getRooms()
        this.onRoomChange(room)
      }
    },
    auth() {
      let user = document.getElementById("authUser").value
      this.server = new WebSocket("wss://nane.tada.team/ws?username=" + user)
      this.$store.commit("auth", user)
      this.toggleVisibleAuth()
    },
    onRoomChange: function (room) {
      if (this.showNewRoom) this.showNewRoom = false
      this.$store.commit("changeRoom", room)
      this.getHistory(room)
      if (!this.showChat) this.showChat = true
    },
  },
  created() {
    this.getRooms()
    this.server = new WebSocket("wss://nane.tada.team/ws?username="+this.$store.state.user)
    this.server.onmessage = (evt) => {
      console.log("SERVER MESSAGE")
      let json = JSON.parse(evt.data)
      if (json.room === this.$store.state.roomCurrent) this.chat.push(this.getRecord(json))
    }

    this.server.onopen = (evt) => {
      console.log("START WORK")
      console.log(evt)
    }

    this.server.onerror = (evt) => {
      console.log("SERVER ERROR")
      console.log(evt)
    }
  }
}
</script>

<style>
  .chat {
    width: 1340px;
    height: 800px;

    margin: 0 auto;
    padding: 25px;

    display: grid;
    grid-template-columns: 300px 810px 230px;
    grid-template-rows: 150px 560px 100px;
    justify-content: center;

    background-color: gainsboro;
    border-radius: 50px;
  }

  .chat__head {
    display: block;
    width: 795px;

    grid-row: 1;
    grid-column: 2 / 3;

    position: relative;
    top: -10px;
  }
</style>