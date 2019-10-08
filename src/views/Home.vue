<template>
  <div class="container">
    <h3>Select two blackhearts</h3>
    <div class="row">
      <div class="col-xs-2" v-for="blackheart in blackhearts" v-bind:key="blackheart.name">
          <input style="display:none" type="checkbox" :id="blackheart.name" :value="blackheart.name" v-model="checkedNames" 
                :disabled="checkedNames.length > 1 && !checkedNames.includes(blackheart.name)">
          <label :for="blackheart.name">
            <img class="profile-pic" 
              :class="[{selected: checkedNames.includes(blackheart.name)}, {greyed: checkedNames.length == 2 && !checkedNames.includes(blackheart.name)}]" 
              :src="getProfilePic(blackheart.name)" /> 
            <br /> 
            {{blackheart.name}} 
          </label>
      </div>
    </div>

    <span v-if="checkedNames.length == 2">
      <h3>Select which tropes to use</h3>
      <ul>
          <li v-for="trope in tropes" v-bind:key="trope.type">
              <input type="checkbox" :id="trope.type" :value="trope" v-model="checkedTropes">
              <label :for="trope.type">{{trope.type}}</label>
          </li>
      </ul>
    </span>

    <span v-if="checkedTropes.length > 0">
      <h3>Our story goes as follows....</h3>
      <p v-text="getStory()" />
    </span>
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
  }, 
  methods: {
    getProfilePic(name) {
      return `/img/profile/${name.toLowerCase()}.jpg`;
    },
    getStory() {
      var story = [...this.checkedTropes].sort((a, b) => a.order - b.order)
                  .map((trope, i) => i % 2 == 0 ? trope.text.format(this.checkedNames[0], this.checkedNames[1]) : trope.textAmbiguous)
                  .join(', and ');
      return story.charAt(0).toUpperCase() + story.slice(1) + '.';
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .profile-pic {
    margin: 10px;
  }
  .selected {
    outline-style: solid;
    outline-color: #4CAF50;
    outline-width: 5px;
  }

  .greyed {
    opacity: 0.4;
    filter: alpha(opacity=40);
  }
</style>
