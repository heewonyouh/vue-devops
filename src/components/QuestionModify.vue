<template>
  <div>
    <div v-if="!submitted">
      <form
        action=""
        method="post"
        id="_frmForm"
        name="frmForm"
        @submit.prevent="modifyArticle"
      >
        <table class="content_table">
          <colgroup>
            <col style="width: 30%;" />
            <col style="width: 70%;" />
          </colgroup>
          <tr>
            <th>이름</th>
            <td>
              <input
                data-msg="이름"
                type="text"
                name="name"
                id="_name"
                v-model=getProfile
                style="width: 30%;"
                disabled
              />
            </td>
          </tr>
          <tr>
            <th>제목</th>
            <td>
              <input
                data-msg="제목"
                type="text"
                name="subject"
                id="subject"
                size="20"
                v-model="subject"
                style="width: 30%;"
              />
            </td>
          </tr>
          <tr>
            <th>내용</th>
            <td>
              <textarea
                data-msg="내용"
                name="content"
                id="content"
                v-model="content"
                style="width: 30%; height:100px"
              />
            </td>
          </tr>
          <tr></tr>
          
          <tr>
            <td colspan="2" style="height: 50px; text-align: center;">
              <button type="submit" name="button">수정하기</button>
            </td>
          </tr>
        </table>
      </form>
    </div>

    <div v-else>
      <h4>성공적으로 질문을 수정하였습니다!</h4>
      <button class="btn btn-success" v-on:click="goArticle">
        돌아가기
      </button>
    </div>
  </div>
</template>

<script>
import http from '../http-common4'
import { mapGetters, mapState } from 'vuex'
export default {
  name: 'add-customer',
  data() {
    return {
      idx:0,
      subject:"",
      content:"",
      submitted : false
    }
  },
  props: ["id"],
  methods: {
      retrieveArticles() {
      http
        .get("/findArticleById/"+this.idx)
        .then(response => (
            this.subject = response.data.subject,
            this.content = response.data.content
            )
            
            )
        .catch(() => {
          this.errored = true;
        })
        .finally(() => (this.loading = false));
    },
    modifyArticle() {
      if (this.subject == '') {
        alert('제목을 입력하세요')
        return
      }
      if (this.content == '') {
        alert('내용을 입력하세요')
        return
      }

      http
        .post('/updateArticle/', {
            idx:this.idx,
          subject: this.subject,
          writer: this.getProfile,
          content: this.content,
        })
        .then((response) => {
          if (response.data.state == 'succ') {
            alert('글수정을 완료하였습니다.')
          } else {
            alert('글수정을 완료하지 못하였습니다..')
          }
        })
      this.submitted = true
    },
    goArticle(){
        this.$router.push("/question/view/"+this.idx)
    }
  },
  computed: {
      ...mapGetters(['isAuthenticated', 'isProfileLoaded','getProfile']),
      ...mapState({
        authLoading: state => state.auth.status === 'loading'
       // ,uname: state => `${state.user.getProfile}`,
      }),
    // loading: function () {
    //   return this.authStatus === 'loading' && !this.isAuthenticated
    // }
  },
  mounted() {
    this.idx = this.id;
    this.retrieveArticles();
  }
}
</script>

<style>
.submitform {
  max-width: 300px;
  margin: auto;
}
</style>
