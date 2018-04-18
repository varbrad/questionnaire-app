<template>
  <section class="container" key="1">
    <h1>Activity Consent</h1>
    <ol>
      <li>I have received and read the provided Participant Information Sheet.</li>
      <li>The activity has been fully explained, and I understand what my participation will involve.</li>
      <li>I understand that my participation is entirely voluntary, and that I am free to withdraw at any time, without giving reason.</li>
      <li>I understand how the data I provide will be used and stored in the research process.</li>
    </ol>
    <div class="toolbar">
      <nuxt-link to="/quiz">I agree to participate</nuxt-link>
    </div>
  </section>
</template>

<script>
export default {};
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

.container > ol > li {
  padding: 1rem;
}

.container > .toolbar {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem;
}

.container > .toolbar > a {
  transition: color 300ms ease-out, transform 150ms ease-out;
  color: orangered;
  padding: 1rem;
  border: 1px solid orangered;
  border-radius: 3px;
  font-size: 1.1rem;
  font-weight: bold;
  text-decoration: none;
  background-color: white;
  transform: scale(1);
}

.container > .toolbar > a:hover {
  background-color: orangered;
  color: white;
  transform: scale(1.1);
  text-decoration: underline;
}
</style>
