<template>
  <div>
    <ul>
        <li v-for="blackheart in blackhearts" v-bind:key="blackheart">
            <input type="checkbox" :id="blackheart" :value="blackheart" v-model="checkedNames" 
                :disabled="checkedNames.length > 1 && !checkedNames.includes(blackheart)">
            <label :for="blackheart">{{blackheart}}</label>
        </li>
        <div v-html="fanfiction[0].text"></div>
    </ul>
  </div>
</template>

<script>
import fanfiction from "../assets/fics";

export default {
  name: 'Home',
  props: {
    msg: String
  },
  data: function () {
    return {
      fanfiction: fanfiction.list,
      blackhearts: ['Beth', 'Billie', 'Cass', 'Emma', 'Garvey' ],
      checkedNames: []
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
