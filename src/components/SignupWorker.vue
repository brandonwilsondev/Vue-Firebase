<template>
  <v-container class="loginCard">
    <v-toolbar
      color="primary lighten-2"
      dark
      justify="center"
      max-width="450px"
      flat
    >
      <v-row justify="space-around">
        <v-toolbar-title><h1>Signup For Workers</h1></v-toolbar-title>
      </v-row>
    </v-toolbar>
    <v-card>
      <v-card-text>
        <v-form @submit="signup" v-model="valid" class="card-panel">
          <v-text-field
            type="Email"
            v-model="email"
            :rules="emailRules"
            label="E-mail:"
            required
          ></v-text-field>
          <v-text-field
            type="password"
            v-model="password"
            label="Password:"
            :rules="[v => !!v || 'Password is required']"
            required
          ></v-text-field>
          <v-text-field
            name="confirmPassword"
            label="Confirm Password"
            id="confirmPassword"
            type="password"
            :rules="[v => !!v || 'Confirm Password is required', comparePasswords]"
            v-model="passwordConfirm"
          ></v-text-field>
          <v-text-field
            type="alias"
            v-model="alias"
            label="First Name:"
            :counter="15"
            :rules="nameRules"
            required
          ></v-text-field>
          <v-select
            v-model="specialty"
            :items="items"
            label="Specialty"
            :rules="[v => !!v || 'Specialty is required']"
            data-vv-name="items"
            required
          ></v-select>
          <v-select
            v-model="experience"
            :items="experiences"
            label="Experience:"
            :rules="[v => !!v || 'Experience is required']"
            data-vv-name="experience"
            required
          ></v-select>
          <v-text-field
            type="number"
            v-model="zip"
            label="Zip Code:"
            :rules="zipCodeRules"
            required
          ></v-text-field>

          <p class="red--text text-center" v-if="feedback">{{ feedback }}</p>
          <v-card-actions class="text-center">
            <div class="flex-grow-1"></div>
            <v-btn :disabled="!valid" @click="signup" color="orange"> Submit </v-btn>
            <!-- <v-btn @click="signupWithGoolge" color="primary lighten-2" dark tile> Signup With Google </v-btn> -->
          </v-card-actions>
        </v-form>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script>
import db from "@/firebase/init";
import slugify from "slugify";
import firebase from "firebase";

export default {
  metaInfo: {
    title: 'Need Work Sign up now at Specialty Stars and start working tomorrow!',
    titleTemplate: 'Specialty Stars',
     htmlAttrs: {
        lang: 'en',
        amp: true
      }
  },
  name: "SignupWorkers",
  data() {
    return {
      valid: true,
      dialog: false,
      name: null,
      contactInfo: null,
      email: null,
      password: null,
      passwordConfirm: null,
      alias: null,
      feedback: null,
      slug: null,
      isWorker: true,
      isWorkerProfile: false,
      zip: "",
      specialty: "",
      experience: "",
      workDur: "",
      strenghts: "",
      skills: "",
      equipment: "",
      travel: "",
      myCred: null,
      experiences: [
        "Helper",
        "Apprentice",
        "Journeyman",
        "Master",
        "Supervisor"
        ],
      items: [
        "Agricultural",
        "Air Conditioning",
        "Auto Paint & Body",
        "Auto Mechanics",
        "Auto Detailing",
        "Brick Pavers",
        "Brick & Block Masonry",
        "Carpentry (rough and finish)",
        "Cabinetry",
        "Construction Cleaning",
        "Concrete Cutting and Drilling",
        "Concrete Placing & Finishing",
        "Dishwashing",
        "Demolition",
        "Delivery/Driver",
        "Drywall",
        "Electrical",
        "Flooring",
        "Fence",
        "Fire & Security Alarm",
        "Fire Protection",
        "Glass & Windows",
        "General Construction",
        "Granite & Stone Fabrication",
        "Golf Course and Greens keeper",
        "Handyman",
        "Home Remodeling",
        "Housekeeping/ Maid service",
        "Irrigation/Sprinkler",
        "Insulation",
        "Janitorial",
        "Landscaping",
        "Marine",
        "Mechanical Systems",
        "Moving",
        "Painting",
        "Paving & Roadwork",
        "Pest Control",
        "Plumbing",
        "Pool Service",
        "Pool & Spa installation",
        "Roofing",
        "Stucco & Plastering",
        "Tile & Marble",
        "Tree Work",
        "Warehouse"
      ],
      // isWorker: [{ id: 1, name: "isWorker", checked: true }],
      // isEmployer: [{ id: 2, name: "isEmployer", checked: true }],
      emailRules: [
        v => !!v || "E-mail is required",
        v => /.+@.+\..+/.test(v) || "E-mail must be valid"
      ],
      nameRules: [
        v => !!v || 'First Name is required',
        v => (v && v.length <= 15) || 'First Name must be less than 15 characters',
      ],
      zipCodeRules: [
        v => !!v || 'ZIP Code is required',
        v => (v && v.length === 5) || 'ZIP Code must be 5 digits',
      ]
    };
  },

  methods: {
    // signupWithGoogle() {
    //   firebase.auth().signInWithPopup(new firebase.auth.GoogleAuthProvider())
    //   .then(function(response) {
    //     console.log(response)
    //   })
    // },
    signup() {    
      // if (
      //   this.alias &&
      //   this.email &&
      //   this.password &&
      //   this.comparePasswords === true &&
      //   this.isWorker
      // ) {
      this.slug = slugify(this.alias, {
        replacement: "-",
        remove: /[$*_+~.()'"!\-:@]/g,
        lower: true
      });
      let ref = db.collection("users").doc(this.slug);
      ref.get().then(doc => {
        if (doc.exists) {
          this.feedback = "This alias already exists";
        } else {
          // this alias does not yet exists in the db
          firebase
            .auth()
            .createUserWithEmailAndPassword(this.email, this.password)
            .then(cred => {
              this.myCred = cred;
              db.collection("users")
                .doc(cred.user.uid)
                .set({
                  email: this.email,
                  isWorker: this.isWorker,
                  alias: this.alias,
                  user_id: cred.user.uid,
                  zip: this.zip,
                  specialty: this.specialty,
                  experience: this.experience,
                  isWorkerProfile: this.isWorkerProfile
                });
              db.collection("specialistProfile")
                .doc(cred.user.uid)
                .set({
                  email: this.email,
                  isWorker: this.isWorker,
                  alias: this.alias,
                  user_id: cred.user.uid,
                  zip: this.zip,
                  specialty: this.specialty,
                  experience: this.experience,
                  isWorkerProfile: this.isWorkerProfile
                });
            })

            .then(() => {
              this.myCred.user
                .sendEmailVerification()
                .then(() => {
                  // Go to login page and set configuration in vuex store
                  this.$store.commit("setConfig", {
                    sendVerify: true,
                    newEmail: this.email,
                    newPassword: this.password
                  });
                  this.$router.push({ name: "Login" });
                })
                .catch(err => {
                  this.feedback = err.message;
                });
            })
            .catch(err => {
              this.feedback = err.message;
            });
        }
      });
        // }
        // .then (() => {
        //   firebase
        //   .firestore()
        //   createSpecialistProfile(this.users.user_id)
        // }
      // } else {
      //   this.feedback = "Please enter data in all fields";
      // }
    }
  },
  computed: {
    comparePasswords() {
      return this.password === this.passwordConfirm
        ? true
        : "Passwords don't match";
    }
  },
  beforeCreate() {
    firebase
      .auth()
      .signInAnonymously()
      .catch(function(error) {
        // Handle Errors here.
        var errorCode = error.code;
        var errorMessage = error.message;
        alert(
          "ErrorCode: " + errorCode + "\n" + "ErrorMessage: " + errorMessage
        );
      });
  }
};
</script>
