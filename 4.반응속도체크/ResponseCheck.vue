<template>
  <div>
    <div id="screen" :class="state" @click="onClickScreen">{{ message }}</div>
    <!-- 아래 div처럼 단순히 감싸는 용도로 사용하는 경우엔 div 대신 template을 사용할 수 있다. -->
    <div v-show = "result.length"> <!-- v-show는 태그가 존재하나 display:none 처리가 되지만, v-if의 경우 해당 태그 부분이 주석처리가 된다. -->
      <div>평균 시간: {{ average }}ms</div>
      <button @click="onReset">리셋</button>
    </div>
  </div>
</template>
<script>
  let startTime = 0;
  let endTime = 0;
  let timeout = null;
  export default {
    data() {
      return {
        result: [],
        state: 'waiting',
        message: '클릭해서 시작하세요',
      };
    },
    computed: { //일반적으로 data를 가공해서 사용할 때 사용. 사용하게 되면 캐싱이 되어 성능에 영향을 줌.
      average() {
        return this.result.reduce((acc, cur) => acc + cur, 0) / this.result.length || 0;
      }  
    },
    methods: {
      onReset() {
        this.result = [];
      },
      onClickScreen() {
        if (this.state === 'waiting') {
          this.state = 'ready';
          this.message = '초록색이 되면 클릭하세요.';
          timeout = setTimeout(() => {
            this.state = 'now';
            this.message = '지금 클릭!';
            startTime = new Date();
          }, Math.floor(Math.random() * 1000) + 2000); //2~3초
          return;
        }
        if (this.state === 'ready') {
          clearTimeout(timeout);
          this.state = 'waiting';
          this.message = '너무 성급하시군요! 초록색이 된 후에 클릭하세요.';
          return;
        }
        if (this.state === 'now') {
          endTime = new Date();
          this.state = 'waiting';
          this.message = '클릭해서 시작하세요.';
          this.result.push(endTime - startTime);
          return;
        }
      },
    },
  };
</script>
<style scoped>
  #screen {
    width: 300px;
    height: 200px;
    text-align: center;
    user-select: none;
  }
  #screen.waiting {
    background-color: aqua;
  }
  #screen.ready {
    background-color: red;
    color: white;
  }
  #screen.now {
    background-color: greenyellow;
  }
</style>
