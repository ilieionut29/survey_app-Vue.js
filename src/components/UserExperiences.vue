<template>
    <section>
        <div class="submited">
            <h3>Submitted experiences</h3>
            <button @click="loadExperinces" type="button" class="btn btn-secondary btn-lg btn-block"> Load submitted experincess</button>
        </div>

        <div class="errors">
            <p v-if="isLoading"> Loading...</p>
            <p v-else-if="!isLoading && error"> {{error}} </p>
            <p v-else-if="!isLoading && (!results || results.length === 0)">
                No stored experinces found. Start adding some experiences first.
            </p>
            <ul v-else>
                <app-survey-result
                    v-for="result in results"
                    :key="result.id"
                    :firstName="result.firstName"
                    :lastName="result.lastName"
                    :age="result.age"
                    :rating="result.rating"
                ></app-survey-result>
            </ul>
        </div>
    </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue'

export default {
    data() {
        return {
            results: [],
            isLoading: false,
            error: null
        }
    },
    methods: {
        loadExperinces() {
            this.isLoading = true;
            this.error = null;
            fetch('https://survey-igi29.firebaseio.com/survey.json')
                .then((response) => {
                    if (response.ok) {
                        return response.json()
                    }
                })
                .then((data) => {
                    this.isLoading = false;
                    const results = [];
                    for (const id in data) {
                        results.unshift({
                            id: id,
                            firstName: data[id].firstName,
                            lastName: data[id].lastName,
                            age: data[id].age,
                            rating: data[id].rating
                        })
                    }
                    this.results = results;
                })
                .catch((error) => {
                    console.log(error);
                    this.isLoading = false;
                    this.error = "Failed to fetch data - please try again later!"
                })
        }
    },
    components: {
        appSurveyResult: SurveyResult
    },
    mounted() {
        this.loadExperinces();
    }
}
</script>

<style scoped>
.submited {
    margin: 2rem auto;
    text-align: center;
}

.errors {
    margin: 2rem auto;
    text-align: center;
}

.btn-block {
    max-width: 300px;
    margin: 0px auto;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>