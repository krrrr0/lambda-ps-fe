<template>
<main class="container">
  <div class="row flex-lg-row-reverse align-items-center g-5 py-5">
    <div class="col-10 col-sm-8 col-lg-6">
    </div>
    <div class="col-lg-6">
      <h1 class="display-5 fw-bold lh-1 mb-3">{{ message }}</h1>
      <p class="lead">야매로 만든 온라인 저지 시스템입니다. <br>현재 Python, C, C++ 의 3가지 언어를 지원하고 있습니다.</p>
    </div>
  </div>
</main>
</template>


<script>
import Vue from 'vue'

export default Vue.extend({
  data() {
    return {
      message: '갱신중...'
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
        this.message = '로그인해서 시작하세요!';
        this.$nuxt.$emit('auth', false);
      } else {
        this.message = '안녕하세요, ' + content.data.nickname + '님!';
        this.$nuxt.$emit('auth', true);
      }
    } catch (e) {
      this.message = '서버 오류!';
      this.$nuxt.$emit('auth', false);
    }
  }
})
</script>
