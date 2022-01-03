<template>
<main class="container">
<div>
  <h2>리더보드</h2>
  <table class="table mt-3">
      <thead>
        <tr>
          <th>순위</th>
          <th>닉네임</th>
          <th>푼 문제</th>
          <th>점수</th>
        </tr>
      </thead>
      <tr v-for="(item, index) in items" :key="item.nickname">
        <td>{{index + 1}}</td>
        <td><NuxtLink :to="`/u/${item.username}`">{{item.nickname}}</NuxtLink></td>
        <td><span v-html="item.num_solved"></span></td>
        <td><span v-html="item.score"></span></td>
      </tr>
  </table>

</div>
</main>
</template>


<script>
import Vue from 'vue'

export default Vue.extend({
  data() {
    return {
      message: '갱신중...',
      items: [
          {
              'username': '',
              "nickname": '갱신중...',
              'num_solved': '갱신중...',
              'score': '갱신중...'
          }
      ]
    }
  },
  async mounted() {
    try {
      const response = await fetch('http://127.0.0.1:8000/info', {
        method: 'GET',
        headers: {'Content-Type': 'application/json'},
        credentials: 'include',
      })

      const content = await response.json();

      console.log(content)

      if (content.result == 'error') {
        this.message = '로그인되지 않았습니다!';
        this.$nuxt.$emit('auth', false);
      } else {
        this.message = '안녕하세요, ' + content.data.nickname + '님!';
        this.$nuxt.$emit('auth', true);
      }
    } catch (e) {
      this.message = '서버 오류!';
      this.$nuxt.$emit('auth', false);
    }
  

    try {
      const response = await fetch('http://127.0.0.1:8000/rank', {
        method: 'GET',
        headers: {'Content-Type': 'application/json'},
        credentials: 'include',
      })

      const content = await response.json();

      console.log(content)

      if (content.result == 'error') {
        this.message = '갱신 실패!';
      } else {
        this.items = content.data;
      }
    } catch (e) {
      this.message = '서버 오류!';
    }

    


  }
})
</script>
