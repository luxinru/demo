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
    HelloWorld
  },

  methods: {
    async test() {
      const res = await axios({
        method: "post",
        url: "http://localhost:3000/pdf-download",
        headers: { "Content-Type": "application/json" },
        // responseType: "blob",
        data: {
          url: "https://bbs.hupu.com/vote"
        }
      });

      console.log("resresresres", res);

      const self = this;

      const reader = new FileReader();

      const blob = new Blob([res.data], {
        type: "application/pdf"
      });

      reader.onloadend = function(e) {
        console.log("eeee", e);
        const a = document.createElement("a");
        const blob = self.getBlob(e.target.result);
        console.log("URL.createObjectURL(blob)", URL.createObjectURL(blob));
        a.href = URL.createObjectURL(blob);

        a.download = "test_2.pdf";
        document.body.appendChild(a);
        a.click();
        document.body.removeChild(a);
      };

      reader.readAsDataURL(blob);
    },

    getBlob(base64) {
      return this.b64toBlob(this.getData(base64), this.getContentType(base64));
    },

    getData(base64) {
      return base64.substr(base64.indexOf("base64,") + 7, base64.length);
    },

    getContentType(base64) {
      return /data:([^;]*);/i.exec(base64)[1];
    },

    b64toBlob(b64Data, contentType, sliceSize) {
      console.log("contentType", contentType);
      console.log("sliceSize", sliceSize);
      contentType = contentType || "";
      sliceSize = sliceSize || 512;

      var byteCharacters = atob(b64Data);
      var byteArrays = [];

      for (
        var offset = 0;
        offset < byteCharacters.length;
        offset += sliceSize
      ) {
        var slice = byteCharacters.slice(offset, offset + sliceSize);

        var byteNumbers = new Array(slice.length);
        for (var i = 0; i < slice.length; i++) {
          byteNumbers[i] = slice.charCodeAt(i);
        }

        var byteArray = new Uint8Array(byteNumbers);

        byteArrays.push(byteArray);
      }

      var blob = new Blob(byteArrays, { type: contentType });
      return blob;
    }
  }
};
</script>
