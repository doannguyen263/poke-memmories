<template>
  <section class="interact__wrap">
    <div class="container">

      <div class="row g-2 g-xl-3 " :class=" `row-col-${column}`">
        <div class="el-col col-3" v-for="(card, index) in cardsContext" :key="index">
          <CardFlip 
          :ref="`card-${index}`"
          :imgBackFaceUrl="`img/${card}.png`" 
          :card="{index, value : card}"
          :rules="rules"
          @onFlip="checkRule($event)"
          />
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import CardFlip from './CardView.vue';
  export default {
    name: 'InteractView',
    props:{
      column : {
        type: Number,
      },
      cardsContext : {
        type : Array,
        default(){
          return [];
        }
      }
    },
    data(){
      return{
        rules: [],
        isDisabled : false,
      }
    },
    methods:{
      checkRule( card ){
        // console.log(card);
        if( this.rules.length < 2){
          this.rules.push(card)
          this.$refs[`card-${this.rules[0].index}`][0].onDisableCard();
        }

        if( this.rules.length === 2 && this.isDisabled == false){
          this.isDisabled = true;
          // check
          if( this.rules[0].value === this.rules[1].value){
            console.log('right');

            setTimeout(()=> {
              this.$refs[`card-${this.rules[0].index}`][0].onDisableCard();
              this.$refs[`card-${this.rules[1].index}`][0].onDisableCard();
              // reset rule
              this.rules = []
              this.isDisabled = false;
            },800)

          }else if( this.rules[0].value !== this.rules[1].value ){
            console.log('wrong');
            
            setTimeout(()=> {
              this.$refs[`card-${this.rules[0].index}`][0].onDisableEnableCard();
              this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
              this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
              // reset rule
              this.rules = [];
              this.isDisabled = false;
            },800)
          }else return false;

        }

        const checkFinish = document.querySelectorAll('.card:not(.disabled)');
        console.log(checkFinish.length);
        if( checkFinish.length == 1){
         
          setTimeout(()=>{
            this.$emit('onFinish')
          },1000)
        }
     
      }
    },
    components:{
      CardFlip,
    }
  }
</script>

<style lang="css" scoped>
  .interact__wrap{
    position: relative;
    padding: 50px 0;
  }
 
</style>
