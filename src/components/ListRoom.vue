<template>
  <div class="rooms">
    <h2 class="rooms__head">
      Открытые чаты
    </h2>
    <ul class="room-list">
      <li class="room-list__el" v-for="room in rooms" v-bind:key="room.name" @click="onRoomClick(room.name)">
        {{ room.name }}
      </li>
    </ul>
    <button class="rooms__room-new btn" @click="onNewRoomClick">
      Новый чат
    </button>
  </div>
</template>

<script>


export default {
  name: "ListRoom",
  props: {
    rooms: Array
  },
  methods: {
    onRoomClick: function(room) {
      this.$emit("room-change", room)
    },
    onNewRoomClick: function () {
      this.$emit("open-new-room")
    }
  }
}
</script>

<style>
  .rooms {
    width: 300px;

    display: flex;
    flex-wrap: wrap;

    grid-column: 1;
    grid-row: 1/4;
  }

  .rooms__head {
    width: 170px;
  }

  .room-list {
    width: 260px;
    height: 569px;

    display: flex;
    flex-wrap: wrap;
    align-items: flex-start;
    align-content: flex-start;

    margin-left: 25px;
    padding-top: 15px;

    overflow-y: scroll;
  }

  .room-list__el {
    width: 230px;
    margin-bottom: 10px;

    overflow-wrap: break-word;
  }

  .rooms__room-new {
    width: 160px;
    height: 60px;

    margin: 0 auto;
  }

</style>