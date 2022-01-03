<template> 
<main class="container">
<div> 
  <h2>문제 둘러보기</h2> 

  <p class="pt-5">총 {{problems.length}}개의 문제가 등록되어 있습니다.</p>
  <NuxtLink role="button" class="btn btn-primary mt-3" to="/p/create">새 문제 만들기</NuxtLink>

  <table class="table mt-3">
    <thead>
      <tr>
        <th>#</th>
        <th>문제 제목</th>
        <th>설명</th>
        <th>만든 사람</th>
        <th>푼 사람</th>
        <th>풀어보기</th>
      </tr>
    </thead>
    <tr v-for="(item, index) in problems" :key="item">
      <td>{{index + 1}}</td>
      <td><span v-html="item.title"></span></td>
      <td><span v-html="item.description"></span></td>
      <td><NuxtLink :to="`/u/${item.author}`">{{item.author}}</NuxtLink></td>
      <td>{{item.success_num}}명</td>
      <td><NuxtLink role="button" class="btn btn-primary btn-sm" :to="`/p/${item.id}`">풀어보기</NuxtLink></td>

    </tr>
  </table>
</div>
</main> 
</template> 


<script> 
export default {
  data() {
    return {
      message: '갱신중...',
      problems: []
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
        this.$nuxt.$emit('auth', false);
      } else {
        this.$nuxt.$emit('auth', true);
      }
    } catch (e) {
      this.$nuxt.$emit('auth', false);
    }

    const response = await fetch('http://127.0.0.1:8000/p', {
      method: 'GET',
      headers: {'Content-Type': 'application/json'}
    })

    const content = await response.json();

    console.log(content)

    if (content.result == 'error') {
      // this.message = content.message;
    } else {
      this.message = content.data;
      this.problems = content.data;
    }

  } 


}
</script>
