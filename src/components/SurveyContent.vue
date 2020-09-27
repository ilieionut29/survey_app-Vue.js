<template>
  <section>
    <div class="box">
      <span class="question-title">How was your experience on my website?</span>
      <div class="base-card">
        <form @submit.prevent="submitSurvey">
          <label for="name" class="label-control"> {{firstNameError}} </label> <br>
            <input
              type="text" 
              autocomplete="off"
              name="firstName" 
              id="firstName"
              class="input-control"
              placeholder="Enter your first name"
              v-model.trim="firstName"
            > 
          <br>
    
          <label for="name" class="label-control"> {{lastNameError}} </label> <br>
            <input
              type="text"
              autocomplete="off"
              name="lastName" 
              id="lastName"
              class="input-control"
              placeholder="Enter your last name"
              v-model.trim="lastName"
            > 
          <br>
        
            <label for="age" class="label-control"> {{ageError}} </label> <br>
              <input
                type="number" 
                autocomplete="off"
                name="age" 
                id="age" 
                class="input-control" 
                placeholder="Enter your age"
                v-model.trim="enteredAge"
              >
            <br>

          <b-form-group class="space">
            <span class="label-control">How was your experience on my website?</span> <br>
            <b-form-radio type="radio" size="sm" class="radio-control" name="rating" id="raatingHilarious" value="hilarious 😐" v-model="chosenRatting">Hilarios</b-form-radio>
            <b-form-radio type="radio" size="sm" class="radio-control" name="rating" id="ratingBored" value="bored 🙃" v-model="chosenRatting">Bored</b-form-radio>
            <b-form-radio type="radio" size="sm" class="radio-control" name="rating" id="ratingAttractive" value="attractive 🥰" v-model="chosenRatting">Attractive</b-form-radio>
            <b-form-radio type="radio" size="sm" class="radio-control" name="beautiful" id="raatingBeautiful" value="beautiful 🥳" v-model="chosenRatting">Beautiful</b-form-radio>
            <span v-if="invalidInput" class="label-control select-error"> {{errorSelect}} </span>
          </b-form-group>
          <p v-if="error"> {{error}} </p>

          <div class="button-control">
            <button type="submit" class="btn btn-success">Submit</button>
            <button type="reset" @click="resetForm" class="btn btn-dark">Reset</button>
          </div>
        </form>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  data() {
    return {
      firstName: "",
      firstNameError: "First name",
      lastName: "",
      lastNameError: "Last name",
      enteredAge: "",
      ageError: "Age",
      chosenRatting: null,
      invalidInput: false,
      errorSelect: "",
      error: null,
    }
  },
  methods: {
     resetForm() {
      this.firstName = ""
      this.lastName = ""
      this.enteredAge = ""
      this.chosenRatting = null
      this.firstNameError = "First name"
      this.lastNameError = "Last name"
      this.ageError = "Age"
      this.invalidInput = false;
    },
    
    submitSurvey() {
      function hasNumber(myString) {
        return /\d/.test(myString);
      }

      let ageToInt = parseInt(this.enteredAge);

      this.firstNameError = "First name"
      this.lastNameError = "Last name"
      this.ageError = "Age"
      if (this.firstName === "") {
        this.firstNameError = "❌ - First name is required!"
        return;
      } else if (hasNumber(this.firstName)) {
        this.firstNameError = "❌ - Your first name should not contain any numbers!"
        return;
      } else if (this.lastName === "") {
        this.lastNameError = "❌ - Last name is required!"
        return;
      } else if (hasNumber(this.lastName)) {
        this.lastNameError = "❌ - Your last name should not contain any numbers!"
        return;
      } else if(this.enteredAge === "") {
        this.ageError = "❌ - Age is required!"
        return;
      } else if (ageToInt >= 100 || ageToInt <= 7) {
        this.ageError = "❌ - Minimum age is 7 and maximum age is 99!"
        return;
      } else if (!this.chosenRatting) {
        this.invalidInput = true;
        this.errorSelect = "❌ - Your experince is required!"
        return;
      }

      this.invalidInput = false;
      this.error = null;
      
      fetch('https://survey-igi29.firebaseio.com/survey.json', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          firstName: this.firstName,
          lastName: this.lastName,
          age: this.enteredAge,
          rating: this.chosenRatting
        }),
      })
      .then((response) => {
        if(response.ok) {
          // ... 
        } else {
          throw new Error('Could not save data!');
        }
      })
      .catch((error) => {
        this.error = error.message
      })

      this.firstName = ""
      this.lastName = ""
      this.enteredAge= ""
      this.chosenRatting = null
    }
  },
}
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css?family=Merriweather&display=swap");
.box {
  max-width: 570px;
  margin: 2rem auto;
  border: 1px solid rgba(34, 47, 62,1.0);
  background:rgba(131, 149, 167,0.8);
  border-radius: 5px;
}

.box:hover {
  box-shadow: 0px 0px 7px rgba(34, 47, 62,1.0);
}

.base-card {
  background-color: #f1f2f6;
  max-width: 5570px;
  padding: 12px 12px;
  border-radius: 0px 0px 5px 5px;
}

.question-title {
  color:#192a56;
  font-size: 14px;
  display: block;
  font-weight: 550;
  height: 50px;
  line-height: 50px;
  margin-left: 20px;
}

.label-control {
  font-family: "Merriweather", serif;
  font-size: 12.5px;
  margin-top: 15px;
  margin-left: 7px;
  margin-bottom: 5px;
}

.input-control {
  width: 98%;
  height: 35px;
  margin-left: 5px;
  padding: 0px 5px;
  font-family: "Merriweather", serif;
  font-weight: 550;
  font-size: 13px;
  border: 1px solid #7f8c8d;
  border-radius: 7px;
}
.input-control:hover {
  box-shadow: 0px 0px 7px  #7f8c8d;
}
.input-control:focus {
  box-shadow: 0px 0px 7px  #7f8c8d;
  outline: none;
}

.radio-control {
  margin-left: 8px;
  font-size: 13px;
}
.space {
  margin-top: 15px;
}

.button-control {
  display: flex;
  justify-content: space-around;
  margin-bottom: 20px;
}




input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
input[type=number] {
  -moz-appearance: textfield;
}
</style>
