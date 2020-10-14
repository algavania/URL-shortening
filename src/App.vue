<template>
  <div id="app">
    <Navbar />
    <Home />
    <Footer />
  </div>
</template>

<script>
import Navbar from "@/components/Navbar";
import Footer from "@/components/Footer";
import Home from "@/views/Home";

export default {
  name: "App",
  components: {
    Navbar,
    Footer,
    Home,
  },
};
</script>

<style>
:root {
  --red: hsl(0, 87%, 67%);
  --cyan: hsl(180, 66%, 49%);
  --cyan-hover: hsl(180, 55%, 63%);
  --dark-violet: hsl(257, 27%, 26%);
  --gray: hsl(0, 0%, 75%);
  --gray-bg: #f0f1f6;
  --grayish-violet: hsl(257, 7%, 63%);
  --very-dark-blue: hsl(255, 11%, 22%);
  --very-dark-violet: hsl(260, 8%, 14%);
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Poppins", sans-serif;
  transition: 0.3s;
}
</style>