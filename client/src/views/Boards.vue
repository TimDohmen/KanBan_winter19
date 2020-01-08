<template>
  <div class="boards">
    WELCOME TO THE BOARDS!!!
    <!-- <form @submit.prevent="addBoard">
      <input type="text" placeholder="title" v-model="newBoard.title" required>
      <input type="text" placeholder="description" v-model="newBoard.description">
      <button type="submit">Create Board</button>
    </form>-->
    <button @click="addBoard">Create Board</button>
    <button @click="logout()">Logout</button>

    <div v-for="board in boards" :key="board._id">
      <router-link :to="{name: 'board', params: {boardId: board._id}}">{{board.title}}</router-link>
      <button @click="removeBoard(board._id)">X</button>
    </div>
  </div>
</template>

<script>
import NotificationService from "../NotificationService.js";
export default {
  name: "boards",
  mounted() {
    this.$store.dispatch("getBoards");
  },
  data() {
    return {
      newBoard: {
        title: "",
        description: ""
      }
    };
  },
  computed: {
    boards() {
      return this.$store.state.boards;
    }
  },
  methods: {
    async addBoard() {
      let boardInfo = await NotificationService.inputData("Enter Board Info");
      if (boardInfo) {
        this.$store.dispatch("addBoard", boardInfo);
        this.newBoard = { title: "", description: "" };
      }
    },
    async removeBoard(boardId) {
      if (
        await NotificationService.confirmAction(
          "Do you really wanna delete dis board"
        )
      ) {
        this.$store.dispatch("removeBoard", boardId);
        return;
      }
    },
    async logout() {
      if (
        await NotificationService.confirmAction(
          "are you sure you wanna log out"
        )
      ) {
        this.$store.dispatch("logout");
      }
    }
  }
};
</script>