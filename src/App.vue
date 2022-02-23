<script setup>
import { ref } from 'vue';

const colors = ['red', 'green', 'blue', 'pink', 'yellow', 'black', 'brown', 'orange', 'purple', 'grey', 'gray', 'cyan', 'coral', 'violet',
  'navy', 'silver', 'skyblue', 'lightblue', 'gold', 'olive', 'aqua', 'aquamarine', 'lime', 'tan', 'maroon', 'beige', 'khaki', 'salmon'];

const colorWord = ref('color 🇺🇸');
const colorWordHead = ref('');
const bgColor = ref('');
const answer = ref('');
const countTime = ref('');
const countPoint = ref(0);  /// score
const todayColor = ref(colors[getTodayColorIndex()]);
const countIncorrect = ref(0);
const isShowHint = ref(false);
const isCorrect = ref(false);
const isCheckedAnswer = ref(false);
const isShowAnswer = ref(false);

console.log(todayColor.value);
// เฉลยของวันนั้นๆ
function getTodayColorIndex() {
  let days = new Date().getDate();
  let index = days % colors.length;
  return index;
}
// เปลี่ยนสีคำว่า colors 
function randColorHeadName() {
  // let colorHead = ['brown', 'pink', 'coral', 'green', 'red']
  let randColorHead = colors[Math.floor(Math.random() * colors.length)];
  colorWordHead.value = randColorHead;
}
//สุ่มคำว่า color แต่ละภาษาที่จะแสดงหน้าเว็บ
function randWebName() {
  let nationColor = ['颜 色 🇨🇳', 'colour 🇬🇧', 'color 🇺🇸', 'farbe 🇩🇪 ', 'couleur 🇫🇷', 'renk 🇹🇷', 'kleur 🇳🇱'];
  let randColorName = nationColor[Math.floor(Math.random() * nationColor.length)];
  colorWord.value = randColorName;
}

setInterval(randColorHeadName, 1000)
setInterval(randWebName, 2000);

// เช็คคำตอบ
function checkGuessWord() {
  if (answer.value === todayColor.value) {
    if (isShowHint.value) {
      countPoint.value += 80;
    } else {
      countPoint.value += 100
    }
    bgColor.value = todayColor.value
    isCorrect.value = true;
    countIncorrect.value = 0;
  } else {
    isCheckedAnswer.value = true;
    countIncorrect.value++
    if (countIncorrect.value >= 6) {
      // show answer
      isCheckedAnswer.value = false;
      isShowHint.value = false;
      isShowAnswer.value = true;
    }
    else if (countIncorrect.value >= 3) {
      isShowHint.value = true;
    }
  }
}

//คำนวณเวลาจนถึงเที่ยงคืนของวันนั้นๆ
function setTime() {
  let crrDate = new Date();
  let tmrDate = new Date(); //1837263
  tmrDate.setHours(24, 0, 0, 0);
  let diffMS = tmrDate.getTime() / 1000 - crrDate.getTime() / 1000;
  let diffHr = Math.floor(diffMS / 3600);
  diffMS = diffMS - diffHr * 3600;
  let diffMi = Math.floor(diffMS / 60);
  diffMS = diffMS - diffMi * 60;
  let diffS = Math.floor(diffMS);
  let result = ((diffHr < 10) ? "0" + diffHr : diffHr);
  result += ":" + ((diffMi < 10) ? "0" + diffMi : diffMi);
  result += ":" + ((diffS < 10) ? "0" + diffS : diffS);
  countTime.value = `${result}`
}
setInterval(setTime, 1000);


</script>
 
<template>
  <div
    id="back"
    class="min-h-screen w-screen w-100 bg-gray-50 py-6 flex flex-col justify-center relative overflow-hidden sm:py-12"
    :style="{ 'background-color': bgColor }"
  >
    <div
      class="dark:bg-slate-800 dark:text-white md:w-96 md:h-auto relative px-6 pt-10 pb-8 bg-white shadow-xl ring-1 ring-gray-900/5 sm:max-w-lg sm:mx-auto sm:rounded-lg sm:px-10"
    >
      <div class="max-w-md mx-auto">
        <div class="flex justify-between">
          <div class="h-6 font-medium text-xl" :style="{ 'color': colorWordHead }">🎨 C O L O R S</div>
          <button type="button" data-modal-toggle="defaultModal">
            <svg
              class="w-6 h-6"
              fill="none"
              stroke="currentColor"
              viewBox="0 0 24 24"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
              />
            </svg>
          </button>
        </div>

        <br />
        <hr />
        <div class="divide-y divide-gray-300/50">
          <div class="dark:text-white py-8 text-base leading-7 space-y-6 text-gray-600">
            <p>Guess {{ colorWord }} !</p>
            <div class="grid grid-flow-col auto-cols-max">
              <div class="relative z-0 mb-6 w-full group md:w-auto">
                <input
                  @keyup="isCheckedAnswer = false;"
                  v-model="answer"
                  type="text"
                  class="block py-2.5 px-0 w-full text-sm text-gray-900 bg-transparent border-0 border-b-2 border-gray-300 appearance-none dark:text-white dark:border-gray-600 dark:focus:border-blue-500 focus:outline-none focus:ring-0 focus:border-blue-600 peer"
                  placeholder="Input Colors"
                  :style="{ 'color': answer }"
                  required
                  :disabled="isCorrect||isShowAnswer"
                />
              </div>
              <div v-show="!isCorrect && !isShowAnswer">
                <button
                  class="relative inline-flex items-center justify-center p-0.5 mb-2 mr-2 overflow-hidden text-sm font-medium text-gray-900 rounded-lg group bg-gradient-to-br from-cyan-500 to-blue-500 group-hover:from-cyan-500 group-hover:to-blue-500 hover:text-white dark:text-white focus:ring-4 focus:ring-cyan-200 dark:focus:ring-cyan-800"
                >
                  <span
                    @click="checkGuessWord"
                    class="px-8 relative px-5 py-2 transition-all ease-in duration-75 bg-white dark:bg-slate-800 rounded-md group-hover:bg-opacity-0"
                  >Check it !</span>
                </button>
              </div>
              <div
                v-show="isCorrect"
                id="toast-success"
                class="flex items-center text-gray-500 bg-white rounded-lg dark:text-gray-400 dark:bg-gray-800"
                role="alert"
              >
                <svg
                  class="w-5 h-5"
                  fill="currentColor"
                  viewBox="0 0 20 20"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    fill-rule="evenodd"
                    d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"
                    clip-rule="evenodd"
                  />
                </svg>
                <i>Correct Answer !</i>
              </div>
              <div
                v-show="isShowAnswer"
                id="toast-success"
                class="flex items-center text-gray-500 bg-white rounded-lg dark:text-gray-400 dark:bg-gray-800"
                role="alert"
              >
                <svg
                  class="w-6 h-6"
                  fill="currentColor"
                  viewBox="0 0 20 20"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    fill-rule="evenodd"
                    d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
                    clip-rule="evenodd"
                  />
                </svg>
                <i>Out of attempt !</i>
              </div>
            </div>

            <div v-show="isCheckedAnswer">
              <span
                v-show="!isCorrect"
                class="bg-red-100 text-red-800 text-xs font-semibold mr-2 px-2.5 py-0.5 rounded dark:bg-red-200 dark:text-red-900"
              >Incorrect ! please try again</span>
              <br />
              <span
                v-show="isShowHint"
                class="bg-red-100 text-red-800 text-xs font-semibold mr-2 px-2.5 py-0.5 rounded dark:bg-red-200 dark:text-red-900"
              >Answer Incorrect ! [ Hint : answer have {{ todayColor.length }} letters ]</span>
            </div>

            <div
              class="p-4 mb-4 text-sm text-green-700 bg-green-100 rounded-lg dark:bg-green-200 dark:text-green-800"
              role="alert"
              id="correct"
              v-show="isCorrect"
            >
              Congratulations, your score is
              <b>{{ countPoint }} point</b>
              <br />You can guess it every day,see you in
              <span
                class="font-medium italic bg-green-200 text-green-800 text-s font-semibold mr-2 px-2.5 py-0.5 rounded dark:bg-green-200 dark:text-green-900"
              >{{ countTime }}</span>
            </div>

            <div
              class="p-4 mb-4 text-sm text-red-700 bg-red-100 rounded-lg dark:bg-red-200 dark:text-red-800"
              role="alert"
              id="correct"
              v-show="isShowAnswer"
            >
            Failure, today color answer is 
            <span
                class="font-medium italic bg-red-200 text-red-800 text-s font-semibold mr-2 px-2.5 py-0.5 rounded dark:bg-red-200 dark:text-red-900"
              ><b>{{ todayColor }}</b></span>
              <br>
              You can guess it every. day see you in
              <span
                class="font-medium italic bg-red-200 text-red-800 text-s font-semibold mr-2 px-2.5 py-0.5 rounded dark:bg-red-200 dark:text-red-900"
              >{{ countTime }}</span>
            </div>

            <div>
              Current Point:
              <span
                class="font-medium bg-blue-100 text-blue-800 text-s font-semibold mr-2 px-2.5 py-0.5 rounded dark:bg-blue-200 dark:text-blue-900"
              >
                <b>{{ countPoint }}</b>
              </span>
              <span
                id="crying"
                style="display:none;"
              >&#128514; You have not scored But you can still answer, fighting !!</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <!-- Modal -->
  <div
    id="defaultModal"
    aria-hidden="true"
    class="hidden overflow-y-auto overflow-x-hidden fixed right-0 left-0 top-4 z-50 justify-center items-center h-modal md:h-full md:inset-0"
  >
    <div class="relative px-4 w-full max-w-2xl h-full md:h-auto">
      <!-- Modal content -->
      <div class="relative bg-white rounded-lg shadow dark:bg-gray-700">
        <!-- Modal header -->
        <div class="flex justify-between items-start p-5 rounded-t border-b dark:border-gray-600">
          <h3 class="text-xl font-semibold text-gray-900 lg:text-2xl dark:text-white">How To Play!</h3>
          <button
            type="button"
            class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm p-1.5 ml-auto inline-flex items-center dark:hover:bg-gray-600 dark:hover:text-white"
            data-modal-toggle="defaultModal"
          >
            <svg
              class="w-5 h-5"
              fill="currentColor"
              viewBox="0 0 20 20"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                fill-rule="evenodd"
                d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
                clip-rule="evenodd"
              />
            </svg>
          </button>
        </div>
        <!-- Modal body -->
        <div class="p-6 space-y-6">
          <p class="text-base leading-relaxed text-gray-500 dark:text-gray-400">
            <b>🎨 Description:</b>
            <br />Guess the COLORS ! . If you are correct you got a point if not point will deduct.
          </p>
          <p class="text-base leading-relaxed text-gray-500 dark:text-gray-400">
            <b>🎨 How to play:</b>
            <br />1. Each day we will randomly pick up 1 color, the player must guess the color to match the random color . Players must type their guessed color in the Input Colors box and then press Check it button.
            <br />2. If the player guesses wrong 3 times, there will be a hint. is the number of characters of that color
            <br />3. You will get the scores when guessing correctly.
            <br /><b>Notice !</b> You can guess it for  5 times . 
          </p>
        </div>
      </div>
    </div>
  </div>
</template>
 
<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
h3 {
  font-family: Quicksand;
}
.alert {
  width: 20%;
  margin: 20px auto;
  padding: 30px;
  position: relative;
  border-radius: 5px;
  box-shadow: 0 0 15px 5px #ccc;
  background: #b2faa7;
}
.close {
  position: absolute;
  width: 30px;
  height: 30px;
  opacity: 0.5;

  right: 15px;
  top: 25px;
  text-align: center;
  font-size: 1.6em;
  cursor: pointer;
}

#app {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: right;

  /* width: auto; */
  min-height: auto;
  overflow-x: hidden;
}

.button {
  appearance: none;
  outline: none;
  border: none;
  background: none;
  cursor: pointer;

  display: inline-block;
  padding: 5px 5px;
  background-image: linear-gradient(to right, #61daf8, #4290f7);
}
</style>