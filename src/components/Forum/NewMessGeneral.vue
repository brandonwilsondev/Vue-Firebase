<template>
  <div class="new-message">
    <br />
    <h3 inset v-if="!user.isLoggedIn">
      <router-link to="/Login">
        <v-btn class="ma-2" tile color="primary lighten-3">
          Login to post
        </v-btn>
      </router-link>
    </h3>
    <div inset v-if="user.isLoggedIn">
      <v-form ref="form" lazy-validation v-model="valid">
        <p>Name: {{ users.alias }}</p>

        <v-text-field
          label="Your Message:"
          type="text"
          name="new-message"
          v-model="newMessage"
        ></v-text-field>
        <v-btn color="orange" tile dark class="mr-4" @click="post">Post</v-btn>
        <p class="red--text" v-if="feedback">{{ feedback }}</p>
      </v-form>
    </div>
  </div>
</template>

<script>
import db from "@/firebase/init";
import firebase from "firebase";
export default {
  name: "NewMessGeneral",
  data() {
    return {
      valid: false,
      alias: null,
      newMessage: null,
      filteredMessage: null,
      feedback: null,
      users: {
        alias: null,
        user_id: null,
        email: null,
      },
    };
  },
  computed: {
    user() {
      return this.$store.state.user;
    },
  },

  methods: {
    // addMessage() {
    //   console.log(this.message, this.users.alias, Date.now());
    // },

    post() {
      var Filter = require("bad-words"),
        filter = new Filter();
      if (this.newMessage) {
        this.filteredMessage = filter.clean(this.newMessage);

        db.collection("genForum").add({
          content: this.filteredMessage,
          alias: this.users.alias,
          timestamp: Date.now(),
          user_id: this.users.user_id,
          email: this.users.email,
        });
        this.newMessage = null;
      } else {
        this.feedback = "You Must enter a message in order to post one.";
      }
    },
  },
  created() {
    // console.log(firebase.auth().currentUser.uid);
    let ref = db
      .collection("users")
      .where("user_id", "==", firebase.auth().currentUser.uid);
    ref.get().then((snapshot) => {
      snapshot.forEach((doc) => {
        this.users = doc.data();
        this.users.id = doc.id;
      });
    });
  },
};
</script>
