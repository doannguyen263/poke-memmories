<template>
  <div class="card" :class="{ 'disabled' : isDisabled }">
    <div class="card__inner" 
    :class="{'is-flipped' : isFlipped}"
    @click="onToggleFlipCard"
    >
      <div class="card__face card__face--front">
        <div class="card__content"></div>
      </div>
      <div class="card__face card__face--back">
        <div class="card__content" :style="{ background : `url( ${require('@/assets/' + imgBackFaceUrl )}) center center / contain no-repeat` }"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CardView',
  props : {
    card:{
      type : [String, Number, Array, Object],
    },
    imgBackFaceUrl: {
      type: String,
      required : true,
    },
    rules: {
      type: Array,
    },
  },
  data(){
    return{
      isDisabled : false,
      isFlipped : false
    }
  },
  methods:{
    onToggleFlipCard(){
      if (this.rules.length >= 2) return;
      if (this.isDisabled) return;
      
        this.isFlipped = !this.isFlipped;
        this.$emit('onFlip',this.card)
    },
    onFlipBackCard(){
      this.isFlipped = false;
    },
    onDisableCard(){
      this.isDisabled = true;
    },
    onDisableEnableCard(){
      this.isDisabled = false;
    }
  }
}
</script>

<style lang="css" scoped>
.card{
  position: relative;
  width: 100%;
  cursor: pointer;
  perspective: 800px;
}
.card.disabled{
  pointer-events: none;
}
.card__inner.is-flipped{
    transform: rotateY(-180deg);
}
.card__inner{
  position: relative;
  width: 100%;
  padding-top: calc(4 / 3 * 100%);
  transition: transform 1s;
  transform-style: preserve-3d;
  
  background: var(--dark);
  border-radius: 8px;
  
}
.card__content{
  width: 100%;
  height: 100%;
}
.card__face{
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  padding: 20px;
  -webkit-backface-visibility: hidden;
  backface-visibility: hidden;
  overflow: hidden;
  box-shadow: 0 3px 18px 3px rgb(0 0 0 / 20%);
  border-radius: inherit;
}
.card__face--front .card__content{
  background: url(../assets/img/icon_back.png) center center / 30% no-repeat;
}
.card__face--back{
  transform: rotateY(180deg);
  background: var(--light);
}
@media (max-width: 575px){
  .card__face {
      padding: 10px;
      box-shadow: 0 3px 10px 3px rgb(0 0 0 / 20%);
  }
  .row-col-8 .card__face, .row-col-10 .card__face {
    padding: 2px;
  }
}
</style>
