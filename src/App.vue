<template>
  <div id="app">
    <div v-if="adBlock">
      <div class="container">
        <h2>It looks like you're using an ad-blocker</h2>
        <p>
          We use ads to keep our content happy and free. Please support us by
          whitelisting.
        </p>
        <div class="container text-center text-md-center ad-blocker">
          <h3>TURN OFF YOUR AD-BLOCKER AND RELOAD THE PAGE</h3>
        </div>
      </div>
    </div>
    <div v-else>
      <p>Our Site Content</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      adBlock: true,
    };
  },
  mounted() {
    this.detectAdBlock();
    setInterval(() => {
      this.detectAdBlock();
    }, 5000);
  },
  methods: {
    async detectAdBlock() {
      const adServers = [
        "https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js",

        // Add more ad server URLs here
      ];

      for (const adServer of adServers) {
        try {
          const response = await axios.get(adServer, {
            // This option ensures that the response object contains the final URL
            maxRedirects: 0,
          });

          // Check if the request was redirected
          if (response.request.responseURL !== adServer) {
            this.adBlock = true;
            return;
          }
        } catch {
          this.adBlock = true;
          return;
        }
      }

      this.adBlock = false;
    },
  },
};
</script>

<style>
/* Add your styles here */
</style>
