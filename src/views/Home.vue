<template>
  <div>
    <h3>Select two blackhearts</h3>
    <ul>
        <li v-for="blackheart in blackhearts" v-bind:key="blackheart.name">
            <input type="checkbox" :id="blackheart.name" :value="blackheart.name" v-model="checkedNames" 
                :disabled="checkedNames.length > 1 && !checkedNames.includes(blackheart.name)">
            <label :for="blackheart.name">{{blackheart.name}}</label>
        </li>
    </ul>

    <h3>Select which tropes to use</h3>
    <ul>
        <li v-for="trope in tropes" v-bind:key="trope">
            <input type="checkbox" :id="trope" :value="trope" v-model="checkedTropes">
            <label :for="trope">{{trope}}</label>
        </li>
    </ul>

    <h3>Our story goes as follows....</h3>
  </div>
</template>

<script>
import data from "../assets/data";

export default {
  name: 'Home',
  props: {
    msg: String
  },
  data: function () {
    return {
      tropes: data.tropes,
      blackhearts: data.blackhearts,
      checkedNames: [],
      checkedTropes: []
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
