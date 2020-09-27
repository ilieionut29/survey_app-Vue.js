<template>
    <li>
       <p>
           🌟 <span class="bold-font"> {{firstName}} {{lastName}}</span>, {{age}} years old,  rated website <span :class="ratingClass"> {{rating}}</span>
        </p> 
    </li>
</template>

<script>
export default {
    props: ['firstName', 'lastName', 'age', 'rating'],
    computed: {
        ratingClass() {
            return "bold-font rating--" + this.rating;
        }
    },
}
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css?family=Merriweather&display=swap");
li {
  margin: 1rem 0;
  background-color: #f1f2f6;
  border: 1px solid #a0a5aa;
  padding: 1rem;
  text-align: center;
}

p {
  font-family: "Merriweather", serif;
  font-size: 16px;
  margin: 0.5rem 0;
}

.bold-font {
  font-weight: 700;
}
.rating--hilarious {
  color: #e74c3c;
}
.rating--bored {
  color: #e67e22;
}
.rating--attractive {
  color: #4cd137;
}
.rating--beautiful {
  color: #0097e6;
}
</style>