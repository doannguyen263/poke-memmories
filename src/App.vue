<template>
  <MainView 
    v-if="statusMatch == 'default'" 
    @onStart="onHandledBeforeStart($event)" 
  />
  <InteractView 
    v-if="statusMatch == 'match'" 
    @onStart="onHandledBeforeStart($event)" 
    @onFinish ="onGetResult"
    :cardsContext="settings.cardsContext" 
    :column="settings.column"
  />
  <ResultView
    v-if="statusMatch == 'result'" 
    @onStartAgain="onStartAgain($event)" 
    :timer="timer"
  />
  <CopyrightView v-if="statusMatch == 'default'" />
</template>

<script>
import MainView from './components/MainView.vue'
import InteractView from './components/InteractView.vue'
import ResultView from './components/ResultView.vue'
import CopyrightView from './components/CopyrightView.vue'

import { shuffled } from './utils/array'

export default {
  name: 'App',
  components: {
    MainView,
    InteractView,
    ResultView,
    CopyrightView
  },
  data(){
    return{
      settings:{
        totalOfBlocks : 0,
        column : 4,
        cardsContext : [],
        startedAt : null,
      },
      statusMatch : 'default',
      timer : 0
    }
  },
  methods : {
    onHandledBeforeStart(config){
      console.log('onHandledBeforeStart', config);
      this.settings.column = Math.sqrt(config.totalOfBlocks);
      
      this.settings.totalOfBlocks = config.totalOfBlocks;
      const firstCards = Array.from({ length : this.settings.totalOfBlocks /2 }, (_, index) => index + 1 );
      const secondCards = [ ...firstCards];
      const cards = [...firstCards,...secondCards];

      // settings
      this.settings.cardsContext = shuffled(shuffled(shuffled(shuffled(cards))));

      console.log(this.settings.cardsContext);

      this.settings.startedAt = new Date().getTime();
      //Ready
      this.statusMatch = 'match';
    },
    onGetResult(){
      
      this.timer = new Date().getTime() - this.settings.startedAt;
      console.log(this.timer);
       //Ready
       this.statusMatch = 'result';
    },
    onStartAgain(){
      console.log('onStartAgain');
      this.statusMatch = 'default';
    }
  }
}
</script>

