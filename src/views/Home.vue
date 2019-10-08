<template>
  <div class="container">
    <h3>Select two blackhearts or <span v-on:click="randomize()" class="btn btn-primary">randomize</span> / <span v-on:click="reset()" class="btn btn-danger">reset</span></h3>
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

    <span v-if="checkedNames.length >= 2">
      <h3>Select which tropes to use</h3>
      <div class="row">
        <div v-for="trope in tropes" v-bind:key="trope.type" >
          <input style="display:none" type="checkbox" :id="trope.type" :value="trope" v-model="checkedTropes">
          <label :for="trope.type">
            <span class="btn btn-primary" :class="{selectedTrope: checkedTropes.includes(trope)}">{{trope.type}}</span>
          </label>
        </div>
      </div>

      <span v-if="checkedTropes.length > 0">
        <h3>Our story goes as follows...</h3>
        <p v-text="getStory()" />
      </span>
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
    },
    reset() {
      this.checkedNames = [];
      this.checkedTropes = [];
    },
    randomize() {
      this.reset();
      var shuffledBlackhearts = [...this.blackhearts].map(blackheart => blackheart.name).sort(() => 0.5 - Math.random());
      this.checkedNames = shuffledBlackhearts.slice(0, 2);

      var shuffledTropes = [...this.tropes].sort(() => 0.5 - Math.random());
      this.checkedTropes = shuffledTropes.slice(0, Math.floor(Math.random() * this.tropes.length) + 1  );
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .container {
    background: 	rgb(255,255,255,.30)
  }

  .profile-pic {
    margin: 10px;
    cursor: pointer;
  }
  .selected {
    outline-style: solid;
    outline-color: #4CAF50;
    outline-width: 5px;
  }
  .greyed {
    opacity: 0.4;
    filter: alpha(opacity=40);
    cursor: not-allowed;
  }
  .selectedTrope {
    background-color: #4CAF50;
  }
  .btn { 
    margin: 5px;
    cursor: pointer;
  }
</style>
