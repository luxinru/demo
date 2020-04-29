<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" @click="test" />
    <HelloWorld msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";
import axios from "axios";

export default {
  name: "Home",
  components: {
    HelloWorld,
  },

  methods: {
    async test() {
      const res = await axios({
        method: "post",
        url: "http://localhost:3000/pdf-download",
        // headers: { "Content-Type": "application/json" },
        // responseType: "blob",
        data: {
          url: "https://www.baidu.com/?token=123456",
        },
      });

      console.log("resresresres", res);

      const reader = new FileReader();

      const blob = new Blob([res.data], {
        type: "application/pdf",
      });

      reader.onerror = function(e) {
        console.log("111111111111", e);
      };

      reader.onloadend = function(e) {
        console.log("eeee", e);
        const a = document.createElement("a");
        a.href = e.target.result;
        a.download = "test_2.pdf";
        document.body.appendChild(a);
        a.click();
        document.body.removeChild(a);
      };

      reader.readAsDataURL(blob);

      console.log("reader", reader);
    },
  },
};
</script>
