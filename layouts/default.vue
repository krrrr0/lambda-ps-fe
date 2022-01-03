<template>
  <div>
    <nav class="navbar navbar-expand-md navbar-dark bg-dark mb-4">
      <div class="container-fluid">
        <NuxtLink to="/" class="navbar-brand">람다 온라인 저지</NuxtLink>

        <div>
          <ul class="navbar-nav me-auto mb-2 mb-md-0" v-if="!auth">
            <li class="nav-item">
              <NuxtLink to="/login" class="nav-link">로그인</NuxtLink>
            </li>
            <li class="nav-item">
              <NuxtLink to="/register" class="nav-link">계정 만들기</NuxtLink>
            </li>
          </ul>

          <ul class="navbar-nav me-auto mb-2 mb-md-0" v-if="auth">
            <li class="nav-item">
              <NuxtLink to="/p/" class="nav-link">문제</NuxtLink>
            </li>
            <li class="nav-item">
              <NuxtLink to="/rank" class="nav-link">리더보드</NuxtLink>
            </li>
            <li class="nav-item">
              <a href="#" class="nav-link" @click="logout">로그아웃</a>
            </li>
          </ul>
        </div>
      </div>
    </nav>
    <Nuxt/>
    <footer class="d-flex flex-wrap justify-content-between align-items-center py-3 my-4 border-top">
      <div class="col-md-4 d-flex align-items-center">
        <span class="text-muted">2021 Lambda.<br>Made with 💖 and ☕ by krrrr</span>
      </div>

    </footer>
  </div>
</template>

<script>
export default {
    name: 'layout',

    data() {
        return {
            auth: false
        }
    },

    mounted() {
        this.$nuxt.$on('auth', auth => {
            this.auth = auth;
        })
    },

    methods: {
        async logout() {
            await fetch('http://127.0.0.1:8000/logout', {
                method: 'POST',
                headers: {'Content-Type': 'application/json'},
                credentials: 'include'
            });

            await this.$router.push('/login');

        }
    }
}
</script>



<style>


.form-signin {
  width: 100%;
  max-width: 330px;
  padding: 15px;
  margin: auto;
}

.form-signin .checkbox {
  font-weight: 400;
}

.form-signin .form-floating:focus-within {
  z-index: 2;
}

.form-signin input[type="email"] {
  margin-bottom: -1px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}

.form-signin input[type="password"] {
  margin-bottom: 10px;
  border-top-left-radius: 0;
  border-top-right-radius: 0;
}

</style>