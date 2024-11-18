<template>
  <div class="pokemon-card" :class="{ hidden: discovered }">
    <div class="image-container">
      <img 
        :src="image" 
        :class="{ blurred: !discovered }" 
        alt="Pokémon"
      />
    </div>
    <div class="details">
      <h3 v-if="discovered" class="anime-title">{{ name }}</h3>
      <div v-else>
        <input 
          v-model="guess" 
          @keyup.enter="checkGuess" 
          class="anime-input" 
          placeholder="¿Quién es este Pokémon?"
        />
        <button @click="checkGuess" class="anime-button">Adivinar</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    name: {
      type: String,
      required: true,
    },
    image: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      guess: '',
      discovered: false,
    };
  },
  methods: {
    checkGuess() {
      if (this.guess.toLowerCase() === this.name.toLowerCase()) {
        this.discovered = true;
        this.$emit('pokemon-discovered');
      } else {
        alert('¡Respuesta incorrecta!');
      }
    },
  },
};
</script>

<style scoped>
.pokemon-card {
  width: 250px;
  height: 320px;
  border-radius: 15px;
  background: linear-gradient(145deg, #fefefe, #f3f3f3);
  box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.2), inset 0px 4px 6px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  position: relative;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  transform: scale(1);
  border: 5px solid #000;
}

.pokemon-card:hover {
  transform: scale(1.05);
  box-shadow: 0px 12px 25px rgba(0, 0, 0, 0.3);
}

.image-container {
  position: relative;
  height: 180px;
  background: radial-gradient(circle, #f1f1f1, #e3e3e3);
  display: flex;
  align-items: center;
  justify-content: center;
}

.image-container img {
  max-width: 100%;
  max-height: 100%;
  filter: brightness(0.8);
  transition: filter 0.3s ease;
}

.image-container img.blurred {
  filter: blur(10px) brightness(0.5);
}

.details {
  padding: 15px;
  text-align: center;
  font-family: 'Impact', 'Arial', sans-serif;
  text-transform: uppercase;
}

.anime-title {
  font-size: 20px;
  color: #ff3c00;
  text-shadow: 2px 2px 0px #000, 4px 4px 0px #ffda00;
  animation: shake 1s infinite alternate;
}

@keyframes shake {
  0% { transform: rotate(-1deg); }
  100% { transform: rotate(1deg); }
}

.anime-input {
  width: 80%;
  padding: 10px;
  border: 2px solid #ff3c00;
  border-radius: 8px;
  font-family: 'Arial', sans-serif;
  font-size: 14px;
  outline: none;
  margin-bottom: 10px;
  background: #fff;
  color: #333;
  box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.15);
}

.anime-input:focus {
  border-color: #ffda00;
}

.anime-button {
  padding: 10px 15px;
  font-size: 14px;
  font-weight: bold;
  color: #fff;
  background-color: #ff3c00;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  text-transform: uppercase;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

.anime-button:hover {
  background-color: #ffda00;
  color: #000;
  transform: scale(1.1);
}
</style>
