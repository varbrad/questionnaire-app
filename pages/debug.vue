<template>
  <section class="container">
    <h1>Total Clips: {{ clips.length }}</h1>
    <h2>Participants: {{ responses.length }}</h2>
    <nuxt-link to="/">Back</nuxt-link>
    <h3>Average Correct: {{ averageCorrect }} out of 10</h3>
    <div class="participants">
      <p v-for="(response, i) in responses" :key="i">Response<span>Correct: {{ response.correct }}/10</span></p>
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
    for (let i = 0; i < localStorage.length; ++i) {
      const key = localStorage.key(i);
      if (key.startsWith("quiz:response:")) {
        responses.push(JSON.parse(localStorage.getItem(key)));
      }
    }
    responses = mapResponses(responses);
    responses.sort((a, b) => b.correct - a.correct);
    //
    // Average correct
    const total = responses.reduce((p, c) => p + c.correct, 0);
    const avg = (total / responses.length).toFixed(2);
    //
    return {
      clips: data.clips,
      responses,
      averageCorrect: avg,
      averageTotal: total
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
</style>
