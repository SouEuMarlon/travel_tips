<template>
  <Header/>
  <main>
    <section class="info__container">
      <h2>Escolha o destino</h2>
      <div class="location__container">
        <label for="location">Destino</label>
        <input
          id="location"
          type="text"
          placeholder="Digite o nome do destino"
          v-model="location"
        />
      </div>
      <h2>Escolha o período</h2>
      
      <div class="date__container">
        <label for="data">Tempo de estádia</label>
        <input
          id="data"
          type="number"
          placeholder="Digite a quantidade de dias"
          v-model="days"
        />
      </div>
      <button @click="run">Buscar</button>
    </section>
    <section>
      <h2>Roteiro</h2>
      <div class="tip__answer-container">
        <p v-if="isLoading" class="loading">
          Aguarde<span class="dot1">.</span><span class="dot2">.</span><span class="dot3">.</span>
        </p>
        <p v-else>
          <span v-html="formattedText"></span>
        </p>
      </div>
    </section>
  </main>

  <Footer/>

</template>

<script>
import { GoogleGenerativeAI } from '@google/generative-ai';
import Footer from './components/Footer.vue';
import Header from './components/Header.vue';

const API_KEY = process.env.API_KEY;

const generativeAI = new GoogleGenerativeAI(API_KEY);

export default {
  components: {
    Header,
    Footer
  },
  data() {
    return {
      text: '',
      location: '',
      days: '',
      formattedText: '',
      isLoading: false
    }
  },
  methods: {
    async run() {
      this.isLoading = true;
      const location = this.location;
      const days = this.days;

      const model = generativeAI.getGenerativeModel({ model: "gemini-pro"});

      const prompt = `Crie um roteiro para uma viagem de exatos ${days} dias na cidade de ${location}, busque por lugares turísticos, lugares mais visitados, seja preciso nos dias de estadia fornecidos e limite o roteiro apenas na cidade fornecida.`

      const result = await model.generateContent(prompt);

      const response = await result.response;

      this.text = response.text();

      this.location = '';
      this.days = '';

      const formattedText = this.text.replace(/(\r\n|\r|\n)/g, '<br>').replace(/(Manhã|Tarde|Noite)/g, '<br>$1<br>').replace(/[:*]/g, '');

      this.isLoading = false;
      
      this.formattedText = formattedText;
    }
  }
}
</script>

<style scoped>

main {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.info__container {
  margin: 2em 0;
  max-width: 700px;
}

.info__container label{
  display: none;
}

.info__container input {
  border: 1px solid #d3d3d3;
  border-radius: 8px;
  padding: 0.6em 1.2em;
  font-size: 1em;
  font-family: inherit;
  width: 100%;
  margin-top: 0.4em;
  text-align: center;
}

.info__container input::placeholder {
  color: #d3d3d3;
}

.info__container input:focus {
  outline: none;
  border: 1px solid #646cff;
  transition: 0.2s ease-in-out;
}


.location__container{
  margin-bottom: 1em;
  margin-top: 0.4em;
}

.date__container{
  margin-bottom: 2em;
  margin-top: 0.4em;
  min-width: 400px;
}

.info__container button {
  border-radius: 8px;
  border: 1px solid #646cff;
  padding: 0.6em 1.2em;
  color: #646cff;
  font-size: 1em;
  font-weight: 500;
  font-family: inherit;
  background-color: #fff;
  cursor: pointer;
  transition: 0.25s ease-in-out;
  display: block;
  margin: 0 auto;
}

.info__container button:hover {
  background-color: #1a1a1a;
  border: 1px solid transparent;
  color: #fff;
}

.tip__answer-container{
  background-color: #f5f5f5;
  border-radius: 8px;
  border: 1px solid #d3d3d3;
  margin-top: 2em;
  max-height: 600px;
  max-width: 700px;
  min-width:500px;
  min-height: 400px;
  overflow-y: auto;
  padding: 1em;
}

.loading {
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
  font-weight: bold;
}

.dot1 {
  animation: blink 1s infinite;
}

.dot2 {
  animation: blink 1s infinite;
  animation-delay: 0.2s;
}

.dot3 {
  animation: blink 1s infinite;
  animation-delay: 0.4s;
}

@keyframes blink {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

@media screen and (max-width: 640px) {

  
  .info__container input {
    width: 100%;
  }
  .date__container {
    min-width: 320px;
  }
  .tip__answer-container {
    min-width: 300px;
    max-width: 320px;
  }
}

</style>
