<template>
  <section class="container" key="2">
    <nuxt-link to="/" class="go-back">Go Back</nuxt-link>
    <h1>Clip #{{clipNumber + 1}}</h1>
    <iframe width="900" height="500"
      :src="'https://www.youtube.com/embed/' + currentClip.url" frameborder="0"
      allow="autoplay; encrypted-media" allowfullscreen>
    </iframe>
    <div class="grid-toolbar">
      <button @click="choice('human')"><img src="/images/human.png"/> Human</button>
      <button @click="choice('ai')"><img src="/images/ai.png"/> AI</button>
      <button @click="choice('unsure')"><img src="/images/unsure.png"/> Unsure</button>
    </div>
  </section>
</template>

<script>
import _ from "lodash";

import data from "@/assets/data";

export default {
  data() {
    const clips = _.sampleSize(data.clips, 10);
    return {
      clipNumber: 0,
      choices: [],
      // 10 random clips from data
      clips
    };
  },
  computed: {
    currentClip() {
      return this.clips[this.clipNumber];
    }
  },
  methods: {
    choice(option) {
      this.choices[this.clipNumber] = {
        id: this.clips[this.clipNumber].id,
        choice: option
      };
      this.clipNumber++;
      if (this.clipNumber >= this.clips.length) {
        // Did all of them
        localStorage.setItem(
          "quiz:response:" + +new Date(),
          JSON.stringify(this.choices)
        );
        this.$router.push("/");
      }
    }
  }
};
</script>

<style>
.container > h1 {
  font-size: 2rem;
  color: black;
  margin: -1rem -1rem 1rem -1rem;
  padding: 1rem;
  background-color: rgba(0, 0, 0, 0.05);
  border-bottom: 1px solid rgba(0, 0, 0, 0.2);
  text-align: center;
}

.container {
  position: relative;
}
.container > .go-back {
  position: absolute;
  top: 0.5rem;
  left: 0.5rem;
  font-size: 0.8rem;
  font-weight: bold;
  border: 1px solid rgba(0, 0, 0, 0.5);
  padding: 0.3rem 0.6rem;
  text-decoration: none;
  color: black;
  border-radius: 3px;
}
.container > .go-back:hover {
  background-color: black;
  color: white;
}

.container > .grid-toolbar {
  display: grid;
  margin-top: 0.5rem;
  grid-gap: 0.5rem;
  grid-template-columns: 1fr 1fr 1fr;
}

.container > .grid-toolbar > button {
  transition: all 300ms ease-out;
  padding-top: 1rem;
  padding-bottom: 1rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border: 3px solid orangered;
  border-radius: 5px;
  background-color: white;
  font-weight: bold;
  font-size: 1.1rem;
  z-index: 1;
}

.container > .grid-toolbar > button > img {
  margin-bottom: 1rem;
}

.container > .grid-toolbar > button:hover {
  background-color: #f1b490;
  transform: scale(1.1);
  z-index: 2;
}
</style>
