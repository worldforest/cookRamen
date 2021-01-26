<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/ramen.jpg" style="height: 30vw;"/>
    <HelloWorld msg="Let's cook Ramen!" />
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";

export default {
  name: "Home",
  components: {
    HelloWorld
  }
};
</script>