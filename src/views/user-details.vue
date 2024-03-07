<template>
  <section v-if="user" class="user-detail-container container">
    <div class="user-details">
      <h1>User Details - {{ user.fullname }}</h1>
      <img :src="user.imgUrl" />
    </div>
    <div class="monthly-income">
      <div>
        <button @click="currMonth--">&lt</button>
        <p>Earning</p>
        <p>₪ {{ totalMonthEarn().paid }}</p>
      </div>
      <p>Monthly</p>
      <div>
        <button @click="currMonth++">></button>
        <p>Unpaid</p>
        <p>₪ {{ totalMonthEarn().arrived }}</p>
      </div>
    </div>
    <div class="total">
      <p>Total monthly earn </p>
      <p>₪ {{ totalMonthEarn().arrived + totalMonthEarn().paid }}</p>
    </div>
    <ul class="students-list">
      <h3>Students</h3>
      <li v-for="student in students" :key="student._id">
        <button @click="openStudentDetails(student)">{{ student.name }}</button>
        <router-link :to="`/student/${student._id}`"></router-link>
      </li>
    </ul>
    <statistic class="stats" />
    <div class="some">
      <p>Total earning this month ₪{{ totalMonthEarn().paid }} from ₪{{ totalMonthEarn().arrived + totalMonthEarn().paid
      }}
      </p>
    </div>
    <div class="student-info">
      <ul v-if="currStudent">
        <li>
          <router-link :to="`/student/${currStudent._id}`">{{ currStudent.name }}</router-link>
          <p>{{ currStudent.classes.length }} classes overall</p>
          <p>{{ classesInMonth(currStudent).length }} classes this month</p>
          <p>{{ paidThisMonth(currStudent).length }} classes paid this month (₪{{ paidThisMonth(currStudent).length *
            currStudent.price }})
          </p>
          <p>{{ arrivedThisMonth(currStudent).length }} classes unpaid this month (₪{{
            arrivedThisMonth(currStudent).length * currStudent.price }})</p>
        </li>
      </ul>
    </div>
  </section>
</template>

<script>
// import {userService} from '../services/user.service'
import statistic from '../cmps/statistic.vue';

export default {
  data() {
    return {
      currStudent: null,
      currMonth: new Date().getMonth()
    }
  },
  async created() {
    this.$store.dispatch({ type: "getStudentByTeacher", teacherId: this.userId });
    this.currStudent = this.students[0]
    // const user = await userService.getById(id)
    // this.user = user
  },
  watch: {
    userId: {
      handler() {
        if (this.userId) {
          this.$store.dispatch({ type: "loadAndWatchUser", userId: this.userId })
        }
      },
      immediate: true,
    },
  },
  computed: {
    user() {
      return this.$store.getters.watchedUser
    },
    userId() {
      return this.$route.params.id
    },
    students() {
      return this.$store.getters.students;
    },
  },
  methods: {
    classesInMonth(student, otherMonth) {
      var month = otherMonth || this.currMonth
      return student.classes.filter(lesson => +lesson.date.split(".")[1] === month + 1)
    },
    paidThisMonth(student) {
      return this.classesInMonth(student).filter(lesson => lesson.status === 'paid')
    },
    arrivedThisMonth(student) {
      return this.classesInMonth(student).filter(lesson => lesson.status === 'arrived')
    },
    openStudentDetails(student) {
      this.currStudent = student
    },
    totalMonthEarn() {
      return this.students.reduce((acc, student) => {
        acc.arrived += this.arrivedThisMonth(student).length * student.price;
        acc.paid += this.paidThisMonth(student).length * student.price;
        return acc;
      }, { arrived: 0, paid: 0 });
    }
  },
  components: {
    statistic
  }
}
</script>