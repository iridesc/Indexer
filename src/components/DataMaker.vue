<template>
  <div>
    <h1>DataMaker</h1>
    <b-container class="bv-example-row">
      <b-row>
        <b-col class="shadow round">
          <!-- Text -->
          <b-form-textarea
            v-model="text"
            placeholder="Item Text..."
            rows="10"
            max-rows="10"
          >
          </b-form-textarea>
        </b-col>
        <!-- Data -->
        <b-col class="shadow round">
          <b-form-textarea
            v-model="dataStr"
            placeholder="JSON Data"
            rows="10"
            max-rows="10"
            disabled
          ></b-form-textarea>
        </b-col>
      </b-row>
    </b-container>
    <br />
    <b-container class="bv-example-row">
      <b-row>
        <!-- splited keys -->
        <b-col class="shadow round speace tbBg">
          <b-badge
            v-for="key in splitedKeys"
            @click="chosedKeys.indexOf(key) == -1 ? choose(key) : remove(key)"
            pill
            class="shadow"
            :variant="chosedKeys.indexOf(key) == -1 ? 'secondary' : 'warning'"
            >{{ key }}
          </b-badge>
        </b-col>
        <!-- chosed keys -->
        <b-col class="shadow round speace tbBg">
          <!-- custom key -->
          <b-form inline>
            <b-input
              class="round"
              size='sm'
              v-model="customKey"
              placeholder="custom key"
            ></b-input>

            <b-button
              class="speace"
              size="sm"
              pill
              variant="primary"
              @click="addCustomKey"
              >Add Key</b-button
            >
            <!-- 向数据库添加数据 -->
            <b-button
              class="speace"
              size="sm"
              pill
              variant="primary"
              @click="pushItem"
              >Push Item</b-button
            >
            <!-- 点击复制数据库数据 -->
            <b-button
              class="speace"
              size="sm"
              pill
              variant="danger"
              v-clipboard:copy="JSON.stringify(this.database, null, 4)"
              v-clipboard:success="onCopySuc"
              v-clipboard:error="onCopyError"
              >Database {{ database.length }}</b-button
            >
          </b-form>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import { Segment, useDefault } from "segmentit";
export default {
  name: "DataMaker",
  data() {
    return {
      text: "",
      customKey: "",
      splitedKeys: [],
      chosedKeys: [],
      data: {
        keysStr: "",
        type: "tetx",
        content: ""
      },
      dataStr: "",
      database: [],
      segment: null
    };
  },
  created() {
    this.segment = useDefault(new Segment());
    this.refreshData();
  },
  watch: {
    text: function() {
      // 更新分词列表
      let newSplitedKeys = this.segment.doSegment(this.text, {
        simple: true
      });
      for (let index = 0; index < newSplitedKeys.length; index++) {
        const key = newSplitedKeys[index];
        if (this.splitedKeys.indexOf(key) == -1) {
          this.splitedKeys.push(key);
        }
      }
      // 根据长度排序
      this.splitedKeys.sort(function sortNumber(b, a) {
        return a.length - b.length;
      });
      // 更新数据
      this.refreshData();
    },
    chosedKeys: function() {
      this.refreshData();
    }
  },
  methods: {
    choose(key) {
      this.chosedKeys.push(key);
    },
    remove(key) {
      this.chosedKeys.splice(this.chosedKeys.indexOf(key), 1);
    },
    addCustomKey(key) {
      if (this.customKey.length > 0) {
        this.chosedKeys.push(this.customKey);
        this.customKey = "";
        this.makeToast("Key added!", "Success", "success");
      } else {
        this.makeToast("Key can't be null", "Error", "danger");
      }
    },
    pushItem() {
      if (this.data.keysStr.length > 0 && this.dataStr.concat.length > 0) {
        this.database.push(JSON.parse(JSON.stringify(this.data)));
        this.text = "";
        this.chosedKeys = [];
        this.splitedKeys = [];
        this.makeToast("成功加入数据库！", "Success", "success");
      } else {
        this.makeToast("Keys/Content cant't be null!", "Error", "danger");
      }
    },

    refreshData() {
      // data
      this.data.content = this.text;
      let keysStr = "";
      this.chosedKeys.forEach(key => {
        keysStr += " " + key;
      });
      this.data.keysStr = keysStr;
      // dataStr
      this.dataStr = JSON.stringify(this.data, null, 4);
    },
    makeToast(content, title, variant = null) {
      this.$bvToast.toast(content, {
        title: title,
        variant: variant
      });
    },
    onCopySuc: function(e) {
      this.makeToast(
        "Database's data Copyed! Paste it into a 'YourDatabasesName.json' file and save it to assets folder!",
        "Success",
        "success"
      );
    },
    onCopyError: function(e) {
      this.makeToast("Failed to copy text!", "Error", "danger");
    }
  }
};
</script>

