<template>
  <div id="app">
    <h2>
      {{ title }}<span class="hl">{{ span }}</span>
    </h2>

    <div class="controls">
      <label for="spacing">{{ Spacing }}</label>
      <input
        id="spacing"
        type="range"
        name="spacing"
        min="10"
        max="200"
        value="10"
        data-sizing="px"
        v-on:change= 'handleUpdate'
        v-on:mousemove= 'handleUpdate'
      />
        <!-- v-on:mousemove= 'handleUpdate' -->

      
      <label for="blur">{{ blur }}:</label>
      <input
        id="blur"
        type="range"
        name="blur"
        min="0"
        max="25"
        value="10"
        data-sizing="px"
      />

      <label for="base">{{ color }}</label>
      <input id="base" type="color" name="base" value="#ffc600" />
    </div>

    <img :src="src" />
  </div>
  
</template>

<script>

export default {
  name: "App",
  data() {
    return {
      src: "https://source.unsplash.com/7bwQXzbF6KE/800x500",
      title: "Update CSS Variables with",
      span: "JSJSJS",
      Spacing: "Spacing:Spacing:",
      blur: "BlurBlur",
      color: "Base ColorColor",
    };
  },
  methods: {
    handleUpdate(){
      
      let input = document.querySelector('.controls #spacing')
      console.log(input)
      const suffix = input.dataset.sizing || '';
      document.documentElement.style.setProperty(`--${input.name}`, input.value + suffix);
    }
  },
}
</script>

<style>
:root {
  --base: #ffc600;
  --spacing: 10px;
  --blur: 10px;
}

img {
  padding: var(--spacing);
  background: var(--base);
  filter: blur(var(--blur));
}

.hl {
  color: var(--base);
}

/*
    misc styles, nothing to do with CSS variables
  */

body {
  text-align: center;
  background: #193549;
  color: white;
  font-family: "helvetica neue", sans-serif;
  font-weight: 100;
  font-size: 50px;
}

.controls {
  margin-bottom: 50px;
}

input {
  width: 100px;
}
</style>
