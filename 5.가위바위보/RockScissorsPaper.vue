<template>
  <div>
    <div id="computer" :style="computedStyleObject"></div>
    <div>
      <button @click="onClickButton('가위')">가위</button>
      <button @click="onClickButton('바위')">바위</button>
      <button @click="onClickButton('보')">보</button>
    </div>
    <div>{{ result }}</div>
    <div>현재 {{ score }}점</div>
  </div>
</template>
<script>
//가위 바위 보 이미지 값
const rspCords = {
  바위: '0',
  가위: '-142px',
  보: '-284px',
};

//가위, 바위, 보 가중치
const scores = {
  가위: 1,
  바위: 0,
  보: -1,
}

//컴퓨터 선택 값
const computerChoice = (imgCode) => {
  return Object.entries(rspCords).find((v) => v[1] === imgCode)[0];
};

//가위 바위 보 이미지 제어
let interval = null;

export default {
  data() {
    return {
      result: '',
      score: 0,
      imgCode: rspCords.바위,
    };
  },
  computed: {
    computedStyleObject() {
      return {
        background: `url(https://en.pimg.jp/023/182/267/1/23182267.jpg) ${this.imgCode} 0`,
      };
    },
  },
  methods: {
    changeHandle() { //중복된 소스를 제거하기 위해 선언
      interval = setInterval(() => {
        switch(this.imgCode) {
          case rspCords.바위:
            this.imgCode=rspCords.가위;
            break;
          case rspCords.가위:
            this.imgCode=rspCords.보;
            break;
          case rspCords.보:
            this.imgCode=rspCords.바위;
            break;
        }
      }, 100);
    },
    onClickButton(choice) {
      clearInterval(interval);
      const myScore = scores[choice];
      const cpuScore = scores[computerChoice(this.imgCode)];
      const diff = myScore - cpuScore;
      switch (diff) {
        case 0:
          this.result = '비겼습니다.';
          break;
        case -1:
        case 2:
          this.result = '이겼습니다.';
          this.score++;
          break;
        default:
          this.result = '졌습니다.';
          this.score--;
      }
      setTimeout(() => {
        this.changeHandle();
      }, 1000);
    },
  },
  //라이프사이클 created, mounted, updated, destroyed 이 4개를 주로 사용
  beforeCreate() {
    console.log('beforeCreate');
  },
  created() {
    //화면에 나타나기 전(컴포넌트가 생성만 된 상태)
    console.log('created');
  },
  beforeMount() {
    console.log('beforeMount');
  },
  mounted() {
    //화면에 나타난 후
    console.log('mounted');
    this.changeHandle();
  },
  beforeUpdate() {
    //console.log('beforeUpdate');
  },
  updated() {
    //console.log('updated');
  },
  beforeDestroy() {
    console.log('beforeDestroy');
    clearInterval(interval); //아래 destroyed에서 처리해도 됨. setInterval, setTimeout에 대한 것을 clear해주지 않으면 메모리 누수가 발생하므로 반드시 해당 처리를 해야함.
  },
  destroyed() {
    console.log('destroyed');
  },
};
</script>
<style scoped>
#computer {
  width: 142px;
  height: 200px;
  background-position: 0 0;
}
</style>
