
<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <AppNavbar />
  <AppLogin />
  <HelloWorld msg="Welcome to Your Vue.js App" />
  <AppScreen />
  <template v-for="city in cityList" v-bind:key="city._id">
    <div>{city.name}</div>
  </template>
</template>

<script>
// https://www.smashingmagazine.com/2020/05/vue-survey-app-firebase-authentication-database/

// Firebase security rules

// https://www.freecodecamp.org/news/how-to-add-authentication-to-a-vue-app-using-firebase/
// Firebase auth
import AppNavbar from "./components/AppNavbar.vue";
import AppLogin from "./components/AppLogin.vue";
import HelloWorld from "./components/HelloWorld.vue";
import AppScreen from "./components/AppScreen.vue";
import firebase from "./firebaseInit.js";
import { getFirestore, collection, addDoc, getDocs } from "firebase/firestore";
const db = getFirestore(firebase);

export default {
  name: "App",
  components: {
    AppNavbar,
    AppLogin,
    HelloWorld,
    AppScreen,
  },
  mounted: async function () {
    console.log(process.env);
    let cities = await this.getCities();
    console.log(cities);
    // await this.createCity("Hobart");
  },
  computed: {
    dog: "hello",
  },
  methods: {
    createCity: async (name) => {
      try {
        console.log("hello creating city");
        const docRef = await addDoc(collection(db, "cities"), { name });
        console.log("Document written with ID: ", docRef.id);
      } catch (error) {
        console.error("Error adding document: ", error);
      }
    },
    getCities: async () => {
      console.log("getting cities");
      const citiesCol = collection(db, "cities");
      const citySnapshot = await getDocs(citiesCol);
      const cityList = citySnapshot.docs.map((doc) => doc.data());
      console.log(cityList);
      return cityList;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
