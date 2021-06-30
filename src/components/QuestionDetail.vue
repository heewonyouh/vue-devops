<template>
  <div>
    <div v-if="!submitted">
      <form
        action=""
        method="post"
        id="_frmForm"
        name="frmForm"
        @submit.prevent="addEmployee"
      >
        <table class="content_table">
          <colgroup>
            <col style="width: 30%;" />
            <col style="width: 70%;" />
          </colgroup>
          <tr>
            <th>제목</th>
            <td>
              <!-- 제목 -->
              {{article.subject}}
            </td>
          </tr>
          <tr>
            <th>작성자</th>
            <td>
              <!-- 작성자 -->
              {{article.writer}}
            </td>
          </tr>
          <tr style="height: 100px">
            <th>내용</th>
            <td>
              <!-- 내용 -->
              {{article.content}}
            </td>
          </tr>
          <tr></tr>
          <tr>
            <th>작성일</th>
            {{article.createDate}}
            <td>
              <!-- 작성일 -->
            </td>
          </tr>
         
        </table>
      <button class="btn btn-success" v-on:click="goList">
        돌아가기
      </button>
      <button class="btn btn-success" v-on:click="deleted">
        삭제하기
      </button>
      <button class="btn btn-success" v-on:click="modifyed">
        수정하기
      </button>
      </form>
    </div>

  </div>
</template>

<script>
import http from "../http-common4";
export default {
  name: 'QuestionItem',
  data() {
    return {
      idx : 0,
      article:[]
    };
  },
  props: ["id"],
  methods: {
    selectApt: function(apt) {
      this.$emit('select-apt', apt);
    },
    retrieveArticles() {
      http
        .get("/findArticleById/"+this.idx)
        .then(response => (this.article = response.data))
        .catch(() => {
          this.errored = true;
        })
        .finally(() => (this.loading = false));
    },
    deleted(){
      http
        .post("/deleteArticle/" + this.idx)
        .then(response => {
          if (response.data.state == "succ") {
            alert("글을 삭제하였습니다.");
            this.$router.push("/question");
          } else {
            alert("글을 삭제하지 못하였습니다.");
          }
        })
        .catch(() => {
          this.errored = true;
        })
        .finally(() => (this.loading = false));
    },
    modifyed(){
      this.$router.push("/question/modify/"+this.idx)
    },
    goList(){
        this.$router.push("/question")
    }
  },
  mounted() {
    this.idx = this.id;
    this.retrieveArticles();
  }
};
</script>

<style scoped>
.img-list {
  width: 50px;
}
.mouse-over-bgcolor {
  background-color: lightblue;
}
</style>
