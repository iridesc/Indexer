<template>
  <div>
    <!-- 展示区 -->
    <div v-if="showingKey != ''">
      <div
        v-for="text in DB.DBs[DB.chosedDB]"
        v-if="text.keysString.split(' ').indexOf(showingKey) > -1"
      >
        <b-img
          v-if="text.type == 'img'"
          class="shadow speaceY"
          fluid
          pill
          :src="text.path"
        ></b-img>
        <b-card
          bg-variant="dark"
          text-variant="white"
          class="tb shadow speaceY"
          v-html="hightlight(text.content, showingKey)"
          v-else
        ></b-card>
      </div>
    </div>
    <h1 class="pageCenter textCenter" v-else>
      Indexer
    </h1>

    <!-- 控制区 -->
    <FootBar>
      <template v-slot>
        <!-- 主索引 -->
        <b-button
          v-if="mainIndexOn"
          pill
          class="shadow speace"
          size="sm"
          variant="info"
          v-for="(SIs, mainIndex) in index"
          @click="
            subIndexs = SIs;
            mainIndexOn = false;
            subIndexOn = true;
          "
          >{{ "- " + mainIndex + " -" }}</b-button
        >

        <!-- 子索引 -->
        <b-button
          v-if="subIndexOn"
          pill
          variant="info"
          class="shadow speace"
          v-for="subIndex in subIndexs"
          @click="showingKey = subIndex"
          >{{ subIndex }}</b-button
        ><b-button
          v-if="subIndexOn"
          class="shadow speace"
          pill
          variant="danger"
          @click="
            mainIndexOn = true;
            subIndexOn = false;
            showingKey = '';
          "
          >Back</b-button
        >
      </template>
    </FootBar>
  </div>
</template>

<script>
import FootBar from "./FootBar";

export default {
  name: "Checker",
  components: {
    FootBar
  },
  props: ["DB"],
  data() {
    return {
      mainIndexOn: true,
      index: {},
      subIndexOn: false,
      subIndexs: [],
      showingKey: ""
    };
  },

  created() {
    // 获取所有的关键字
    let allKeysString = "";
    let keys = [];

    this.DB.DBs[this.DB.chosedDB].forEach(text => {
      allKeysString += " " + text.keysString;
    });

    // 除重
    allKeysString.split(" ").forEach(key => {
      if (keys.indexOf(key) == -1 && key.length > 0) {
        keys.push(key);
      }
    });
    // 排序
    keys.sort(function(item1, item2) {
      return item1.localeCompare(item2, "zh-CN");
    });

    // 建立关键字索引
    keys.forEach(key => {
      let indexKey = this.locat(key);
      if (this.index[indexKey] == null) {
        this.index[indexKey] = [key];
      } else {
        this.index[indexKey].push(key);
      }
    });
  },
  methods: {
    hightlight(content, key) {
      return (
        "<p>" +
        content.replace(key, '<span class="tbHl">' + key + "</span>") +
        "</p>"
      );
    },
    locat(key, list = "ABCDEFGHIJKLMNOPQRSTUVWXYZ#".split("")) {
      let latte = list.shift();
      // console.log(key,latte,(key.localeCompare(latte) >= 0) && key.localeCompare(list[0]) < 0);

      if (
        key.spell().toUpperCase() >= latte &&
        key.spell().toUpperCase() < list[0]
      ) {
        // console.log("->", latte);
        return latte;
      } else {
        // console.log("<-", latte, key);
        if (list[0] != null) {
          return this.locat(key, list);
        } else {
          return "#";
        }
      }
    }
  }
};
</script>
