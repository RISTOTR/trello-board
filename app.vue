
import TrelloBoard from './components/TrelloBoard.vue';
<template>
  <div class="p-10 h-[100vh] bg-gray-950 overflow-auto">
    <h1 class="text-4xl text-white flex items-center mb-10">
      <img
      width="80px"
      class="mr-3"
      src="./public/icon-green.png"
      />
      Trello Board with Vue.js
    </h1>
    <TrelloBoard />
  </div>
</template>
