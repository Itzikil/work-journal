<template>
  <header>
    <nav>
      <router-link :to="`/user/${loggedInUser._id}`" class="loggedin-user" v-if="loggedInUser">
        <img :src="loggedInUser.imgUrl" />
        <p>{{ loggedInUser.fullname }}</p>
      </router-link>
      <!-- <router-link to="/">
        <span role="img" aria-label="logo">Home</span>
      </router-link> -->
      <!-- <div> -->
      <router-link to="/today" class="link-container upper-curve">
        <div  :class="['a-container', {active: $route.path === '/today'}]">
          <img src="../assets/imgs/header/today.svg" alt="">
          <p>Today</p>
        </div>
      </router-link>
      <router-link to="/journal" class="link-container">
        <div :class="['a-container', {active: $route.path === '/journal'}]">
          <img src="../assets/imgs/header/calendar.svg" alt="">
          <p>Journal</p>
        </div>
      </router-link>
      <router-link to="/students" class="link-container lower-curve">
        <div :class="['a-container', {active: $route.path === '/students'}]">
          <img src="../assets/imgs/header/students.svg" alt="">
          <p>Students</p>
        </div>
      </router-link>
      <!-- <router-link to="/review">Reviews</router-link> -->
      <!-- <router-link to="/chat">Chat</router-link> -->
      <router-link v-if="!loggedInUser" to="/login">Login / Signup</router-link>
      <!-- <router-link to="/about" class="link-container">
        <div :class="['a-container', {active: $route.path === '/about'}]">
          <img src="../assets/imgs/header/about.svg" alt="">
          <p>About</p>
        </div>
      </router-link> -->
      <!-- </div> -->
    </nav>
  </header>
</template>
<script>
export default {
  computed: {
    loggedInUser() {
      return this.$store.getters.loggedinUser
    },
  }
}
</script>