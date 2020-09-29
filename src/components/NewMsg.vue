<template>
  <form class="msg-new" v-on:submit.prevent="onSubmit">
    <input
        class="msg-new__room"
        v-if="!isChat"
        @input="onRoomInput"
        id="room-name"
        type="text"
        maxlength="34"
        required
        placeholder="Новый чат">
    <textarea class="msg-new__text" id="text-msg" required placeholder="Сообщение">
    </textarea>
    <input class="msg-new__sign" v-if="authorized" id="user" type="text" placeholder="Подпись">
    <button class="msg-new__btn" type="submit">
      Отправить
    </button>
  </form>
</template>

<script>

export default {
  name: "NewMsg",
  props: {
    isChat: Boolean
  },
  methods: {
    onSubmit: function () {
      this.$emit("submit-new-msg")
    },
    onRoomInput: function (evt) {
      let test = this.$store.state.ALLOWED_SYMBOLS.exec(evt.target.value)
      if ( test === null || test[0].length !== evt.target.value.length) {
        evt.target.setCustomValidity("Пожалуйста, используйте только кирилицу, латиницу, цифры и _")
      }  else {
        evt.target.setCustomValidity("")
      }
    },
  },
  computed: {
    authorized: function () {
      return this.$store.state.user === "Anonimus666"
    }
  }
}
</script>

<style>
  .msg-new {
    display: grid;
    grid-template-columns: 826px 215px;
    grid-template-rows: repeat(2, 55px);
    grid-column: 2 / 3;
    grid-row: 3;

    position: relative;
  }

  .msg-new__room {
    height: 40px;

    position: absolute;
    top: -70px;

    grid-row: 1;
    grid-column: 1;
  }

  .msg-new__text {
    height: 80px;

    grid-column: 1;
    grid-row: 1/2;

    resize: none;
  }

  .msg-new__room, .msg-new__text {
    width: 790px;
  }

  .msg-new__btn {
    grid-column: 2;
    grid-row: 2;
  }

  .msg-new__sign {
    width: 165px;
    height: 25px;
    line-height: 45px;
    justify-self: center;

    grid-column: 2;
    grid-row: 1;
  }

  .msg-new__btn, .msg-new__sign {
    position: relative;
    top: -20px;
  }
</style>