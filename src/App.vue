<template>
  <div id="app" class="tbHl bg textCenter">
    <div v-if="settingOn" class=" pageCenter  speace">
      <h1 class="">Indexer</h1>

      <b-card header="Database" bg-variant="dark" text-variant="white" class="tb shadow">
        <b-form-radio
          v-for="(data, name) in DB.DBs"
          v-model="DB.chosedDB"
          name="some-radios"
          :value="name"
          >{{ name }}</b-form-radio
        >
      </b-card>

      <b-button
        class="shadow speace"
        variant="warning"
        pill
        @click="
          settingOn = !settingOn;
          dataMakerOn = !dataMakerOn;
        "
        >DataMaker</b-button
      >
      <b-button
        class="shadow speace"
        variant="warning"
        pill
        @click="settingOn = !settingOn"
        >OK</b-button
      >
    </div>
    <div v-else-if="dataMakerOn">
      <b-button
        class="shadow speace"
        variant="danger"
        pill
        @click="
          settingOn = !settingOn;
          dataMakerOn = !dataMakerOn;
        "
        >Back</b-button
      >

      <DataMaker></DataMaker>
    </div>
    <div v-else>
      <b-button
        class="shadow speace"
        variant="warning"
        pill
        @click="settingOn = !settingOn"
        >{{ DB.chosedDB }}</b-button
      >
      <!-- 检索页面 -->
      <Checker :DB="DB"></Checker>
    </div>
  </div>
</template>

<script>
import Checker from "./components/Checker";
import DataMaker from "./components/DataMaker";

export default {
  name: "App",
  components: {
    Checker,
    DataMaker
  },
  mounted() {
    document
      .querySelector("body")
      .setAttribute("style", "background-color: darkslategray;");
  },
  beforeDestroy() {
    document.querySelector("body").removeAttribute("style");
  },
  created() {
    let DBs = require("./DBs.json");
    for (let index = 0; index < DBs.length; index++) {
      const dbName = DBs[index];
      this.DB.DBs[dbName] = require("./assets/" + dbName + ".json");
    }
  },
  data() {
    return {
      DB: {
        DBs: {},
        chosedDB: null
      },
      settingOn: true,
      dataMakerOn: false
    };
  }
};
</script>

<style>
body {
  background-color: darkslategray;
}

.bg {
  background-color: darkslategray;
}

.tb {
  color: rgb(40, 66, 66);
  font-weight: bold;
}
.tbBg {
  background-color: cadetblue;
  /* background-color:teal; */
  /* background-color: dimgray; */
}
.tbHl {
  color: indianred;
}

.speaceX {
  margin-left: 2%;
  margin-right: 2%;
}

.speaceY {
  margin-top: 2%;
  margin-bottom: 2%;
}

.speace {
  margin: 2%;
}
.round {
  border-radius: 20px;
  padding: 5px;
}
.textCenter {
  text-align: center;
}

.pageCenter {
  position: fixed;
  margin: auto;
  width: 80%;
  top: 25%;
  left: 0;
  right: 0;
}

.shadow {
  box-shadow: black;
}
</style>
