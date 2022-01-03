<template>
<main class="form-signin">
  <form @submit.prevent="submit">
    <!--<img class="mb-4" src="/docs/5.1/assets/brand/bootstrap-logo.svg" alt="" width="72" height="57">-->
    <h1 class="h3 mb-3 fw-normal">로그인하세요.</h1>

    <div class="form-floating">
      <input v-model="username" type="username" class="form-control" id="floatingInput" placeholder="사용자 이름" autofocus>
      <!--<label for="floatingInput">사용자 이름</label>-->
    </div>
    <div class="form-floating">
      <input v-model="password" type="password" class="form-control" id="floatingPassword" placeholder="비밀번호">
      <!--<label for="floatingPassword">비밀번호</label>-->
    </div>

    <button class="w-100 btn btn-lg btn-primary" type="submit">드가자~</button>
  </form>
</main>
</template>


<script>
export default {
    name: "login",
    data() {
        return {
            username: '',
            password: ''
        }
    },
    methods: {
        async submit() {
            await fetch('http://127.0.0.1:8000/login', {
                method: 'POST',
                headers: {'Content-Type': 'application/json'},
                credentials: 'include',
                body: JSON.stringify({
                    username: this.username,
                    password: this.password
                })
            });

            await this.$router.push('/');
        }
    }
}
</script>
