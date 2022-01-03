<template>
<main class="form-signin">
  <form @submit.prevent="submit">
    <!--<img class="mb-4" src="/docs/5.1/assets/brand/bootstrap-logo.svg" alt="" width="72" height="57">-->
    <h1 class="h3 mb-3 fw-normal">계정 만들기</h1>

    <input v-model="username" type="username" class="form-control" placeholder="사용자 이름" autofocus>
    <input v-model="password" type="password" class="form-control" placeholder="비밀번호">
    <input v-model="nickname" type="nickname" class="form-control" placeholder="닉네임">
    <input v-model="bio" type="bio" class="form-control" placeholder="Bio">


    <button class="w-100 btn btn-lg btn-primary" type="submit">제출</button>
  </form>
</main>
</template>


<script>
export default {
    name: "register",
    data() {
        return {
            nickname: '',
            username: '',
            password: '',
            bio: ''
        }
    },
    methods: {
        async submit() {
            await fetch('http://127.0.0.1:8000/user', {
                method: 'POST',
                headers: {'Content-Type': 'application/json'},
                body: JSON.stringify({
                    nickname: this.nickname,
                    username: this.username,
                    password: this.password,
                    bio: this.bio
                })
            });

            await this.$router.push('/login');
        }
    }
}
</script>
