<template>
  <section class="container">
    <h1>Total Clips: {{ clips.length }}</h1>
    <h2>Participants: {{ responses.length }}</h2>
    <nuxt-link to="/">Back</nuxt-link>
    <h3>Average Correct: {{ averageCorrect }} out of 10</h3>
    <div class="participants">
      <p v-for="(response, i) in responses" :key="i">Response<span>Correct: {{ response.correct }}/10</span></p>
    </div>
    <div class="grid">
      <div class="col">
        <h1>AI ID</h1>
        <p v-for="(ai, key) in ais" :key="key">{{ key }} <span>{{ ai.correct }} / {{ ai.total }}</span><span>{{ ((ai.correct * 100) / ai.total).toFixed(1) }}%</span></p>
      </div>
      <div class="col">
        <h1>Human ID</h1>
        <p v-for="(human, key) in humans" :key="key">{{ key }} <span>{{ human.correct }} / {{ human.total }}</span><span>{{ ((human.correct * 100) / human.total).toFixed(1) }}%</span></p>
      </div>
    </div>
  </section>
</template>

<script>
import data from "@/assets/data";

function mapResponses(responses) {
  return responses.map(d => {
    let correct = 0;
    d.forEach(answer => {
      // Get the answer they chose
      const opt = answer.choice;
      const id = answer.id;
      // Find that ID in the array
      const vid = data.clips.find(o => o.id === id);
      if (vid.type === opt) correct++;
    });
    return {
      data: d,
      correct
    };
  });
}

export default {
  data() {
    let responses = [];
    let ais = {};
    let humans = {};
    for (let i = 0; i < localStorage.length; ++i) {
      const key = localStorage.key(i);
      if (key.startsWith("quiz:response:")) {
        responses.push(JSON.parse(localStorage.getItem(key)));
      }
    }
    responses = mapResponses(responses);
    responses.sort((a, b) => b.correct - a.correct);
    // Go thru every response
    responses.forEach(r => {
      // Go through every data point
      r.data.forEach(answer => {
        const choice = answer.choice;
        const actual = data.clips.find(v => v.id === answer.id);
        if (actual.type === "ai") {
          // AI
          if (!ais[actual.id]) ais[actual.id] = { total: 0, correct: 0 };
          ais[actual.id].total++;
          if (actual.type === choice) ais[actual.id].correct++;
        } else {
          // Human
          if (!humans[actual.id]) humans[actual.id] = { total: 0, correct: 0 };
          humans[actual.id].total++;
          if (actual.type === choice) humans[actual.id].correct++;
        }
      });
    });
    //
    // Average correct
    const total = responses.reduce((p, c) => p + c.correct, 0);
    const avg = (total / responses.length).toFixed(2);

    return {
      clips: data.clips,
      responses,
      averageCorrect: avg,
      averageTotal: total,
      ais,
      humans
    };
  }
};
</script>

<style scoped>
.participants {
  max-height: 300px;
  overflow-x: hidden;
  overflow-y: auto;
}

.participants > p {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  padding: 0.5rem;
}

.participants > p:nth-child(odd) {
  background-color: #f0f0f0;
}

.participants > p > span {
  color: green;
}

.grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
}

.grid > .col {
  padding: 0.5rem;
  border: 1px solid #f0f0f0;
  margin: 0.5rem;
  border-radius: 3px;
}

.grid > .col > h1 {
  font-size: 1.5rem;
  background-color: #f0f0f0;
  padding: 0.5rem;
  font-weight: 400;
  text-align: center;
}

.grid > .col > p {
  display: grid;
  grid-template-columns: auto 1fr 1fr;
  font-size: 0.6rem;
  font-weight: bold;
}

.grid > .col > p > span {
  font-size: 1rem;
  font-weight: 400;
  text-align: right;
}

.grid > .col > p:nth-child(even) {
  background-color: #fafafa;
}
</style>
