<template>
  <div>
    <div>당첨 숫자</div>
    <div id="result">
      <lotto-ball v-for="ball in winBalls" :key="ball" :number="ball"></lotto-ball>
    </div>
    <div v-if="bonus">보너스</div>
    <lotto-ball v-if="bonus" :number="bonus"></lotto-ball>
    <button v-if="redo" @click="onClickRedo">한번 더!</button>
  </div>
</template>
<script>
import LottoBall from './LottoBall';

const timeouts = [];

function getWinNumbers() {
  const candidate = Array(45)
    .fill()
    .map((v, i) => i + 1);
  const shuffle = [];
  while (candidate.length > 0) {
    shuffle.push(candidate.splice(Math.floor(Math.random() * candidate.length), 1)[0]);
  }
  const bonusNumber = shuffle[shuffle.length - 1];
  const winNumbers = shuffle.slice(0, 6).sort((a, b) => a - b);
  return [...winNumbers, bonusNumber];
}

export default {
  components: {
    LottoBall, // == 'lotto-ball': LottoBall //kebab과 Pascal 호환이 되는 경우엔 생략가능. 실무에서도 다르게 쓰는 경우가 거의 없음.
  },
  data() {
    return {
      winNumbers: getWinNumbers(), //미리 숫자 선정
      winBalls: [], //시각적인 용도로 볼을 하나씩 추가하기 위해 사용
      bonus: null,
      redo: false,
    };
  },
  computed: {},
  methods: {
    showBalls() {
      for (let i = 0; i < this.winNumbers.length - 1; i++) {
        timeouts.push(
          setTimeout(() => {
            this.winBalls.push(this.winNumbers[i]);
          }, (i + 1) * 1000),
        );
      }
      timeouts.push(
        setTimeout(() => {
          this.bonus = this.winNumbers[this.winNumbers.length - 1];
          this.redo = true;
        }, this.winNumbers.length * 1000),
      );
    },
    onClickRedo() {
      this.winNumbers = getWinNumbers();
      this.winBalls = [];
      this.bonus = null;
      this.redo = false;
    },
  },
  mounted() {
    //화면에 나타난 후
    this.showBalls();
  },
  beforeDestroy() {
    timeouts.forEach((t) => {
      clearTimeout(t);
    });
  },
  watch: {
    //값을 감시할 data를 함수명으로 사용. value:현재값, oldValue:이전값
    //객체의 경우 참조값이므로 이전값과 현재값이 같을 수 있으므로 되도록이면 사용x
    //하지만, watch는 최대한 사용을 자제하고 대체할 수 있는 방법이 있으면 그 방법을 사용.
    //watch는 거의 최후의 수단으로 사용한다라고 생각하는게 좋음.(리액트도 포함)
    //(watch로 인하여 데이터가 예상치못하게 변경되는 등 여러가지 사이드이펙트 발생가능성이 높음)
    bonus(value, oldValue) {
      console.log(value, oldValue);
      if (value === null) {
        this.showBalls();
      }
    },
  },
};
</script>
<style></style>
