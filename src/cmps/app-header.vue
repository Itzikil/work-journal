<template>
  <header>
    <nav>
      <section class="loggedin-user" v-if="loggedInUser">
        <img :src="loggedInUser.imgUrl" />
        <router-link :to="`/user/${loggedInUser._id}`">{{ loggedInUser.fullname }}</router-link>
      </section>
      <!-- <router-link to="/">
        <span role="img" aria-label="logo">Home</span>
      </router-link> -->
      <!-- <div> -->
      <router-link to="/today">Today</router-link>
      <router-link to="/journal">Journal</router-link>
      <router-link to="/students">Students</router-link>
      <!-- <router-link to="/review">Reviews</router-link> -->
      <!-- <router-link to="/chat">Chat</router-link> -->
      <router-link v-if="!loggedInUser" to="/login">Login / Signup</router-link>
      <router-link to="/about">about</router-link>
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