<script setup>
import { ref, computed } from "vue";

const questions = ref([
  {
    question: "Inside which HTML element do we put the JavaScript?",
    answer: 0,
    options: ["<script>", "<js>", "<javascript>"],
    selected: null,
  },
  {
    question: "The external JavaScript file must contain the <script> tag.",
    answer: 1,
    options: ["True", "False", "I Dont Know"],
    selected: null,
  },
  {
    question: 'How do you write "Hello World" in an alert box ?',
    answer: 2,
    options: [
      'msg("Hello World");',
      'alertBox("Hello World");',
      'alert("Hello World");  ',
    ],
    selected: null,
  },
  {
    question: "How do you create a function in JavaScript?",
    answer: 2,
    options: [
      "function:myFunction()",
      "function = myFunction()",
      "function myFunction()",
    ],
    selected: null,
  },
  {
    question: "How to write an IF statement in JavaScript?",
    answer: 0,
    options: ["if (i == 5)  ", "if i == 5 then", "if i = 5"],
    selected: null,
  },
]);

const quizCompleted = ref(false);

const currentQuestion = ref(0);

const clicked = ref(null);

// A computed property is used to declaratively describe a value that depends on other values.
// mengapa disini menggunakan computed property karena disini kita menentukan skor berdasarkan jawaban yang dipilih
// maka setelah memilih dan akan di cek kalau benar maka value skor akan bertambah
const score = computed(() => {
  let value = 0;
  questions.value.map((q) => {
    if (q.selected == q.answer) {
      value = value + 1;
    }
  });
  return value;
});

// fungsi ini digunakan untuk mengambil sekarang soal keberapa berdasarkan variabel current question
const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value];
  question.index = currentQuestion.value;
  question.clicked = clicked.value;
  return question;
});

const setAnswer = (evt) => {
  questions.value[currentQuestion.value].selected = evt.target.value;
  clicked.value = evt.target.value;
  evt.target.value = null;
};

const nextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value = currentQuestion.value + 1;
    clicked.value = null;
  } else {
    quizCompleted.value = true;
  }
};
</script>

<template>
  <body>
    <main class="app">
      <h1 class="sm:text-xl md:text-4xl lg:text-5xl text-center font-sans text-slate-400 mt-5">
        JavaScript Quiz
      </h1>

      <section class="quiz" v-if="!quizCompleted">
        <!-- pembungkus -->
        <div
          class="card h-[40%] sm:w-[80%] md:w-[70%] lg:w-[60%] mt-5 mx-auto bg-slate-800 rounded-md drop-shadow-md p-5"
        >
          <div class="quiz-info flex justify-between m-2">
            <!-- Pertanyaan -->
            <span class="sm:text-xs md:text-xl lg:text-2xl text-2xl text-slate-400">{{
              getCurrentQuestion.question
            }}</span>
            <!-- Score -->
            <span class="sm:text-xs md:text-xl lg:text-2xl text-2xl text-slate-400">
              Your Score : {{ score }} / {{ questions.length }}</span
            >
          </div>

          <div class="options">
            <!-- disini merupakan perulangan untuk memanggil pilihan pilihan pada setiap pertanyan -->
            <!-- dan disini juga dilakukan pengkondisian menggunakan ternary operator -->
            <!-- disini kita memilih apakah option yang dipilih benar atau tidak -->
            <!-- condition ? exprIfTrue : exprIfFalse -->
            <label
              v-for="(option, index) in getCurrentQuestion.options"
              :key="index"
              :class="`option${
                getCurrentQuestion.selected == index
                  ? index == getCurrentQuestion.answer
                    ? 'correct'
                    : 'wrong'
                  : ''
              } ${
                getCurrentQuestion.selected != null &&
                index != getCurrentQuestion.selected
                  ? 'disabled'
                  : ''
              }`"
            >
              <ul>
                <li
                  class="cursor-pointer rounded-md border-2 m-2 p-2 hover:opacity-50 bg-blue-950 "
                  :class="`${
                    getCurrentQuestion.clicked == index ? 'opacity-50' : ''
                  }`"
                >
                  <input
                    type="radio"
                    :name="getCurrentQuestion.index"
                    :value="index"
                    v-model="getCurrentQuestion.selected"
                    :disabled="getCurrentQuestion.selected"
                    @change="setAnswer"
                    class="hidden focus:opacity-50"
                  />
                  <span class="sm:text-lg text-2xl text-white">{{ option }}</span>
                </li>
              </ul>
            </label>
          </div>
          <button
            @click="nextQuestion"
            :disabled="!getCurrentQuestion.selected"
            class="border-2 m-2 rounded-md w-[20%] h-[30%] bg-green-800 disabled:bg-red-900"
          >
            {{
              getCurrentQuestion.index == questions.length - 1
                ? "Finish"
                : getCurrentQuestion.selected == null
                ? "Select an option"
                : "Next Question"
            }}
          </button>
        </div>
      </section>
      <section v-else>
        <div class="end text-center mt-6">
          <h2 class="text-white text-4xl">Quiz is Done</h2>
          <p class="text-white">
            And you got {{ score }} correct out of
            {{ questions.length }} questions
          </p>
        </div>
      </section>
    </main>
  </body>
</template>
