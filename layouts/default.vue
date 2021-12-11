<template>
  <div>
    <Nuxt />
    <hr />
    <p>
      <center>
        Made by mong. 2021/12/10 <b-badge variant="warning">{{counter}}</b-badge>
      </center>
    </p>
  </div>
</template>
<script>
export default {
  data() {
    return {
      counter: 0,
      url: 'https://count.mong.workers.dev/gallery'
    }
  },
  mounted() {
    let vm = this;
      this.$axios.get(this.url).then(function (response) {
        // console.log(response.data.value);
        vm.counter = response.data.value;
      });
  },
}
</script>
<style>
@import url("https://fonts.googleapis.com/css2?family=Prompt:wght@500&display=swap");
body {
  font-family: "Prompt", sans-serif !important;
}
html {
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
}

.button--green {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #3b8070;
  color: #3b8070;
  text-decoration: none;
  padding: 10px 30px;
}

.button--green:hover {
  color: #fff;
  background-color: #3b8070;
}

.button--grey {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #35495e;
  color: #35495e;
  text-decoration: none;
  padding: 10px 30px;
  margin-left: 15px;
}

.button--grey:hover {
  color: #fff;
  background-color: #35495e;
}
</style>
