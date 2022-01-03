<template>
  <main class="container">
    <div>
      <h2>문제 만들기</h2>
      <h3 class="mt-5">기본 정보</h3>
      <input
        class="form-control mt-3"
        type="text"
        placeholder="제목"
        v-model="title"
      />
      <input
        class="form-control mt-3"
        type="text"
        placeholder="설명"
        v-model="description"
      />

      <h3 class="mt-5">테스트케이스 추가하기</h3>
      <p>주의: 빈 칸을 남겨두지 마세요!</p>
      <table class="table mt-3">
        <thead>
          <tr>
            <th>#</th>
            <th>입력</th>
            <th>출력</th>
            <th>설명</th>
            <th></th>
          </tr>
        </thead>
        <tr v-for="(row, index) in rows" :key="index">
          <td>{{ index + 1 }}</td>
          <td>
            <textarea class="form-control" type="text" v-model="row.in" />
          </td>
          <td>
            <textarea class="form-control" type="text" v-model="row.out" />
          </td>
          <td>
            <textarea
              class="form-control"
              type="text"
              v-model="row.description"
            />
          </td>
          <td>
            <button class="btn btn-danger btn-sm" @click="removeRow(index)">
              삭제
            </button>
          </td>
        </tr>
        <tr>
          <td>
            <button class="btn btn-primary btn-sm" @click="addRow">추가</button>
          </td>
          <td></td>
          <td></td>
          <td></td>
        </tr>
      </table>
      <button class="btn btn-primary btn-lg" @click="submit">만들기</button>
    </div>
  </main>
</template> 


<script>
export default {
  data() {
    return {
      message: "갱신중...",
      title: "",
      description: "",
      rows: [
        {
          in: "",
          out: "",
          description: "",
        },
        {
          in: "",
          out: "",
          description: "",
        },
      ],
    };
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
  },
  methods: {
    addRow: function () {
      this.rows.push({
        in: "",
        out: "",
        description: "",
      });
    },
    removeRow: function (index) {
      //console.log(row);
      this.rows.splice(index, 1);
    },
    async submit() {
      try {
        await fetch("http://127.0.0.1:8000/p", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          credentials: "include",
          body: JSON.stringify({
            testcases: this.rows,
            title: this.title,
            description: this.description,
          }),
        });

        await this.$router.push("/p/");
      } catch (e) {
        await this.$router.push("/error");
      }
    },
  },
};
</script>
