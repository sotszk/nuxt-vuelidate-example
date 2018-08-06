<template>
  <div class="container">
    <h2>User Registration Form</h2>
    <UserForm :models="userModels"/>
  </div>
</template>

<script>
import UserForm from '@/components/UserForm'

export default {
  components: { UserForm },

  data () {
    return {
      userModels: {
        name: '',
        age: 20,
        bio: '',
        hobby: '',
        hobby2: '',
      }
    }
  },
}
</script>
<style scoped>
* {
  font-family: 'Do Hyeon', sans-serif;
}

.container {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  margin: 0 auto;
  max-width: 600px;
}

h2 {
  margin-bottom: 20px;
}
</style>
