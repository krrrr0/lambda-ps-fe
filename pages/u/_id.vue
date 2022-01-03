<template> 
<main class="container">
<div> 
  <h2>사용자 정보: {{username}}</h2> 
  <h3 class="pt-5">기본 정보</h3>
  <table class="table mt-3">
    <tr>
      <td>사용자 이름</td>
      <td>{{username}}</td>
    </tr>
    <tr>
      <td>닉네임</td>
      <td>{{nickname}}</td>
    </tr>
    <tr>
      <td>소개</td>
      <td>{{bio}}</td>
    </tr>
    <tr>
      <td>가입 날짜</td>
      <td>{{created}}</td>
    </tr>
  </table>
  
  <h3 class="pt-5">푼 문제 ({{num_solved}}개: {{score}}점)</h3>
  <table class="table mt-3">
    <thead>
      <tr>
        <th></th>
        <th>고유 ID</th>
      </tr>
    </thead>
    <tr v-for="item in solved" :key="item">
      <td>✅</td>
      <td><NuxtLink :to="`/p/${item}`">{{item}}</NuxtLink></td>
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
      username: '갱신중...',
      nickname: '갱신중...',
      created: '갱신중...',
      bio: '갱신중...',
      num_solved: 0,
      score: 0,
      solved: []
    }
  },

  asyncData({params}) { 
    console.log( params ); 
    return { 
      id: params.id 
    }; 
  }, 

  validate ({params}) {
    return /^.+$/.test(params.id); 
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

    const response = await fetch('http://127.0.0.1:8000/user?username=' + this.id, {
      method: 'GET',
      headers: {'Content-Type': 'application/json'}
    })

    const content = await response.json();

    console.log(content)

    if (content.result == 'error') {
      // this.message = content.message;
    } else {
      this.message = content.data;
      this.username = content.data.username;
      this.nickname = content.data.nickname;
      this.created = content.data.created;
      this.bio = content.data.bio;
      this.num_solved = content.data.num_solved;
      this.score = content.data.score;
      this.solved = content.data.solved;
    }

  } 


}
</script>
