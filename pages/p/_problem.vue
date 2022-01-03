<template>
  <main class="container">
    <div>
      <h2>문제 보기: {{ problem.title }}</h2>
      <ul>
        <li>설명: {{ problem.description }}</li>
        <li>만든 날짜: {{ problem.created }}</li>
        <li>
          만든 사람:
          <NuxtLink :to="`/u/${problem.author}`">{{ problem.author }}</NuxtLink>
        </li>
      </ul>

      <h3 class="mt-5">푼 사람: {{ problem.success_num }}명</h3>
      <table class="table mt-3">
        <tr v-for="(item, index) in problem.success" :key="item">
          <td>{{ index + 1 }}번째</td>
          <td>
            <NuxtLink :to="`/u/${item}`">{{ item }}</NuxtLink>
          </td>
        </tr>
      </table>

      <h3 class="mt-5">테스트케이스</h3>
      <table class="table mt-3">
        <thead>
          <tr>
            <th>#</th>
            <th>설명</th>
            <th>입력</th>
            <th>출력</th>
          </tr>
        </thead>
        <tr v-for="(item, index) in problem.testcases" :key="item">
          <td>{{ index + 1 }}</td>
          <td>{{ item.description }}</td>
          <td>{{ item.in }}</td>
          <td>{{ item.out }}</td>
        </tr>
      </table>

      <h3 class="mt-5">풀어보기</h3>
      <p class="mt-3">모든 문제에 대한 제한 시간은 5초입니다.</p>
      <form @submit.prevent="submit">
        <div class="input-group mb-3 mt-3">
          <select class="form-control" v-model="lang" :required="true">
            <option
              v-for="option in options"
              v-bind:value="option.value"
              v-bind:key="option"
            >
              {{ option.text }}
            </option>
          </select>
        </div>
        <textarea
          v-model="code"
          class="form-control"
          style="height: 300px"
          placeholder="여기에 코드를 입력하세요."
        ></textarea>
        <button class="btn btn-primary mt-3" to="/so">몰?루</button>
      </form>

      <h3 class="mt-5">결과: {{ result }}</h3>
      <h5 class="mt-3">출력 (stdout)</h5>
      <textarea
        class="form-control"
        v-model="out"
        style="height: 100px"
        disabled
      ></textarea>
      <h5 class="mt-3">출력 (stderr)</h5>

      <textarea
        class="form-control"
        v-model="err"
        style="height: 100px"
        disabled
      ></textarea>
    </div>
  </main>
</template> 


<script>
export default {
  data() {
    return {
      message: "갱신중...",
      problem: {},
      lang: "python",
      code: "",
      options: [
        { text: "Python 3", value: "python" },
        { text: "C", value: "c" },
        { text: "C++", value: "cpp" },
      ],
      result: "대기...",
      out: "대기...",
      err: "대기...",
    };
  },

  asyncData({ params }) {
    console.log(params);
    return {
      problem: params.problem,
    };
  },

  validate({ params }) {
    return /^.+$/.test(params.problem);
  },

  async mounted() {
    try {
      const response = await fetch("http://127.0.0.1:8000/info", {
        method: "GET",
        headers: { "Content-Type": "application/json" },
        credentials: "include",
      });

      const content = await response.json();

      console.log(content);

      if (content.result == "error") {
        this.$nuxt.$emit("auth", false);
      } else {
        this.$nuxt.$emit("auth", true);
      }
    } catch (e) {
      this.$nuxt.$emit("auth", false);
    }

    const response = await fetch("http://127.0.0.1:8000/p?id=" + this.problem, {
      method: "GET",
      headers: { "Content-Type": "application/json" },
    });

    const content = await response.json();

    console.log(content);

    if (content.result == "error") {
      this.message = content.message;
    } else {
      this.message = "";
      this.problem = content.data;
    }
  },
  methods: {
    async submit() {
      try {
        const response = await fetch("http://127.0.0.1:8000/solve", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          credentials: "include",
          body: JSON.stringify({
            id: this.problem.id,
            lang: this.lang,
            code: this.code,
          }),
        });

        const content = await response.json();
        console.log(content);
        this.message = content;

        if (content.result == "success") {
          this.result = "맞았습니다! 새로고침하세요.";
          this.out = "모든 테스트케이스를 통과했습니다!";
          this.err = "모든 테스트케이스를 통과했습니다!";

        } else if (content.result == "fail") {
          this.result = "틀렸습니다! 다시 시도해 보세요.";
          this.out = content.data.out;
          this.err = content.data.err;
        } else {
          this.result = "오류!";
        }
      } catch (e) {
        this.result = "서버 오류! 관리자에게 문의하세요.";
      }

      // await this.$router.push('/');
    },
  },
};
</script>
