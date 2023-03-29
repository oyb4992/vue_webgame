<template>
  <div class="ball" :style="styleObject">{{ number }}</div>
</template>
<script>
export default {
  name: 'LottoBall',
  //props로 넘어온 값은 변경불가(readonly). 값의 변경이 불가피한 경우 부모의 데이터를 바꾸는 메서드를 자식에게 props로 전달하여 처리해야한다.
  //데이터의 불일치를 방지하기 위함.
  props: {
    number: Number,
  },
  computed: {
    styleObject() {
      if (this.number > 40) {
        return { backgroundColor:'green'};  
      }
      if (this.number <= 10) {
        return { backgroundColor: 'red' };
      }
      if (this.number > 10 && this.number <= 20) {
        return { backgroundColor : 'orange'};
      }
      if (this.number > 20 && this.number <= 30) {
        return { backgroundColor : 'yellow' };
      }
      if (this.number > 30 && this.number <= 40) {
        return {backgroundColor : 'blue'};
      }
    },
  },
  methods: {},
};
</script>
<style>
.ball {
  display: inline-block;
  border: 1px solid black;
  border-radius: 20px;
  width: 40px;
  height: 40px;
  line-height: 40px;
  font-size: 20px;
  text-align: center;
  margin-right: 20px;
}
</style>
