<script setup>
import { ref, computed } from 'vue'

const perguntas = ref([
  {
	pergunta: 'O que é Vue?',
	resposta: 0,
	opcoes: [
		'Um framework',
		'Uma biblioteca',
		'Um tipo de metodo'
	],
	selecionada: null
  },
  {
	pergunta: 'Para que serve o Vuex?',
	resposta: 2,
	opcoes: [
		'Criar arquivo',
		'Visualizar coisas',
		'Gerenciamento de estado'
	],
	selecionada: null
  },
  {
	pergunta: 'O que é o Vue Router?',
	resposta: 1,
	opcoes: [
		'Um objeto',
		'Uma biblioteca de rotas para Vue',
		'Uma classe'
	],
	selecionada: null
  }
])

const quizFinalizado = ref(false)
const perguntaAtual = ref(0)
const pontuacao = computed(() => {
	let valor = 0
	perguntas.value.map(p => {
		if (p.selecionada != null && p.resposta == p.selecionada) {
			console.log('correto');
			valor++
		}
	})
	return valor
})

const obterPerguntaAtual = computed(() => {
	let pergunta = perguntas.value[perguntaAtual.value]
	pergunta.indice = perguntaAtual.value
	return pergunta
})

const definirResposta = (e) => {
	perguntas.value[perguntaAtual.value].selecionada = e.target.value
	e.target.value = null
}

const proximaPergunta = () => {
	if (perguntaAtual.value < perguntas.value.length - 1) {
		perguntaAtual.value++
		return
	}
	
	quizFinalizado.value = true
}
</script>

<template>
	<main class="app">
		<h1>O Quiz</h1>
		
		<section class="quiz" v-if="!quizFinalizado">
			<div class="quiz-info">
				<span class="question">{{ obterPerguntaAtual.pergunta }}</span>
				<span class="score">Pontuação {{ pontuacao }}/{{ perguntas.length }}</span>
			</div>
			
			<div class="options">
				<label 
					v-for="(opcao, indice) in obterPerguntaAtual.opcoes" 
					:for="'opcao' + indice" 
					:class="`option ${
						obterPerguntaAtual.selecionada == indice 
							? indice == obterPerguntaAtual.resposta 
								? 'correta' 
								: 'errada'
							: ''
					} ${
						obterPerguntaAtual.selecionada != null &&
						indice != obterPerguntaAtual.selecionada
							? 'desativada'
							: ''
					}`">
					<input 
						type="radio" 
						:id="'opcao' + indice" 
						:name="obterPerguntaAtual.indice" 
						:value="indice" 
						v-model="obterPerguntaAtual.selecionada" 
						:disabled="obterPerguntaAtual.selecionada"
						@change="definirResposta" 
					/>
					<span>{{ opcao }}</span>
				</label>
			</div>
			
			<button 
				@click="proximaPergunta" 
				:disabled="!obterPerguntaAtual.selecionada">
				{{ 
					obterPerguntaAtual.indice == perguntas.length - 1 
						? 'Finalizar' 
						: obterPerguntaAtual.selecionada == null
							? 'Selecione uma opção'
							: 'Próxima pergunta'
				}}
			</button>
		</section>

		<section v-else>
			<h2>Você terminou o quiz!</h2>
			<p>Sua pontuação é {{ pontuacao }}/{{ perguntas.length }}</p>
		</section>
	</main>
</template>

<style>
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
	font-family: 'Montserrat', sans-serif;
}

body {
	background-color: #271c36;
	color: #FFF;
}

.app {
	display: flex;
	flex-direction: column;
	align-items: center;
	padding: 2rem;
	height: 100vh;
}

h1 {
	font-size: 2rem;
	margin-bottom: 2rem;
}

.quiz {
	background-color: #382a4b;
	padding: 1rem;
	width: 100%;
	max-width: 640px;
}

.quiz-info {
	display: flex;
	justify-content: space-between;
	margin-bottom: 1rem;
}

.quiz-info .question {
	color: #8F8F8F;
	font-size: 1.25rem;
}

.quiz-info.score {
	color: #FFF;
	font-size: 1.25rem;
}

.options {
	margin-bottom: 1rem;
}

.option {
	padding: 1rem;
	display: block;
	background-color: #271c36;
	margin-bottom: 0.5rem;
	border-radius: 0.5rem;
	cursor: pointer;
}

.option:hover {
	background-color: #2d213f;
}

.option.correta {
	background-color: #2cce7d;
}

.option.errada {
	background-color: #ff5a5f;
}

.option:last-of-type {
	margin-bottom: 0;
}

.option.desativada {
	opacity: 0.5;
}

.option input {
	display: none;
}

button {
	appearance: none;
	outline: none;
	border: none;
	cursor: pointer;
	padding: 0.5rem 1rem;
	background-color: #2cce7d;
	color: #2d213f;
	font-weight: 700;
	text-transform: uppercase;
	font-size: 1.2rem;
	border-radius: 0.5rem;
}

button:disabled {
	opacity: 0.5;
}

h2 {
	font-size: 2rem;
	margin-bottom: 2rem;
	text-align: center;
}

p {
	color: #8F8F8F;
	font-size: 1.5rem;
	text-align: center;
}
</style>
