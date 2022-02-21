<template>
  <div class="Word ml-5 mt-2">
    <div v-if="word.hasOwnProperty('length')" class="flex flex-row">
      <div class="border border-slate-300 m-2 w-10 h-10 flex
      justify-center items-center"
           :class="{
              'bg-red-100': isGameOver && !letter.found,
              'bg-green-100': isWin,
            }"
           v-for="letter in word" :key="letter.id">
        <span v-if="letter.found||isGameOver" class="">{{letter.value}}</span>
        <span v-else class="">&nbsp;</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Word',
  props: {
    word: {
      type: Array,
      required: true
    },
    isWin: {
      type: Boolean,
      required: true
    },
    isGameOver: {
      type: Boolean,
      required: true
    }
  },
}
</script>
