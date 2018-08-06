<template>
  <div>
    <div class="form">
      <label>Name
        <input
          type="text"
          v-model.trim="models.name"
          @input="setName($event.target.value)"
        >
        <div v-if="this.$v.models.name.$dirty">
          <div class="error" v-if="!$v.models.name.required">Name is required</div>
          <div class="error" v-if="!$v.models.name.minLength">Name must have at least {{$v.models.name.$params.minLength.min}} letters</div>
          <div class="error" v-if="!$v.models.name.maxLength">Name must have at most {{$v.models.name.$params.maxLength.max}} letters</div>
        </div>
      </label>

      <label>Age
        <select
          v-model="models.age"
          @input="setAge($event.target.value)"
        >
          <option v-for="i in 60" :key="i" :value="i">{{i}}</option>
        </select>
        <div v-if="this.$v.models.age.$dirty">
          <div class="error" v-if="!$v.models.age.required">Age is required</div>
          <div class="error" v-if="!$v.models.age.between">Age must be between 20 and 40</div>
        </div>
      </label>

      <label>Bio
        <textarea
          v-model.trim="models.bio"
          @input="setBio($event.target.value)"
        ></textarea>
      </label>

      <label>Hobby
        <input
          v-model.trim="models.hobby"
          type="text"
          @input="setHobby($event.target.value)"
        >
        <div v-if="this.$v.models.hobby.$dirty">
          <div class="error" v-if="!$v.models.hobby.required">Hobby is required</div>
          <div class="error" v-if="!$v.models.hobby.mustBeCool">Hobby is must start from cool</div>
        </div>
      </label>

      <label>Hobby 2
        <input
          v-model.trim="models.hobby2"
          type="text"
          @input="setHobby2($event.target.value)"
        >
        <div v-if="this.$v.models.hobby2.$dirty">
          <div class="error" v-if="$v.models.hobby2.sameAsHobby">Hobby 2 must not be same as Hobby</div>
        </div>
      </label>

      <div>
        <input @click.prevent="submit" type="submit" name="" value="Submit">
      </div>
    </div>

    <div>
      <h3>RESULTS</h3>
      <div>name: {{ models.name }}</div>
      <div>age: {{ models.age }}</div>
      <div>bio: {{ models.bio }}</div>
      <div>hobby: {{ models.hobby }}</div>
      <div>hobby2: {{ models.hobby2 }}</div>
    </div>
  </div>
</template>

<script>
import { validationMixin } from 'vuelidate'
import { required, minLength, maxLength, between, sameAs, integer } from 'vuelidate/lib/validators'

export default {
  mixins: [ validationMixin ],

  props: ['models'],

  // validateions () {
  //   const validations = {}
  //   if (models.hobby2) {
  //     validations.models.hobby2 = {}
  //   }
  //   return validations
  // },

  validations: {
    models: {
      name: {
        required,
        minLength: minLength(4),
        maxLength: maxLength(20),
      },
      age: {
        required,
        integer,
        between: between(20, 40)
      },
      bio: {
      },
      hobby: {
        required,
        mustBeCool: (value) => /^cool/.test(value)
      },
      hobby2: {
        sameAsHobby: sameAs('hobby')
      }
    }
  },

  mounted () {
    setTimeout(() => {
      console.log(this.$v.models)
    }, 1000)
  },

  methods: {
    submit () {
      this.$v.models.$touch()
      if (this.$v.$invalid) {
        console.log('Error')
      } else {
        console.log('Done!')
      }
    },

    setName (value) {
      this.name = value
      this.$v.models.name.$touch()
    },

    setAge (value) {
      this.age = value
      this.$v.models.age.$touch()
    },

    setBio (value) {
      this.bio = value
      this.$v.models.bio.$touch()
    },

    setHobby (value) {
      this.hobby = value
      this.$v.models.hobby.$touch()
    },

    setHobby2 (value) {
      this.hobby2 = value
      this.$v.models.hobby2.$touch()
    }
  }
}
</script>

<style scoped>
* {
  font-family: 'Do Hyeon', sans-serif;
}

label {
  display: block;
  margin-bottom: 20px;
}

.form {
  border: 4px solid #333;
  padding: 20px;
  margin-bottom: 30px;
}

.error {
  color: #a00;
}
</style>
