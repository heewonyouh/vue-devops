<template>
  <div>
    <div v-if="!submitted">
      <form
        action=""
        method="post"
        id="_frmForm"
        name="frmForm"
        @submit.prevent="addArticle"
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
              <button type="submit" name="button">글쓰기</button>
            </td>
          </tr>
        </table>
      </form>
    </div>

    <div v-else>
      <h4>성공적으로 질문을 등록하였습니다!</h4>
      <button class="btn btn-success" v-on:click="newArticle">
        새로운 질문 등록하기
      </button>
      <button class="btn btn-success" v-on:click="goList">
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
      cname: '',
      subject: '',
      content: '',
      submitted : false
    }
  },
  methods: {
    addArticle() {
      if (this.subject == '') {
        alert('제목을 입력하세요')
        return
      }
      if (this.content == '') {
        alert('내용을 입력하세요')
        return
      }

      http
        .post('/addArticle', {
          subject: this.subject,
          writer: this.getProfile,
          content: this.content,
        })
        .then((response) => {
          if (response.data.state == 'succ') {
            alert('글작성을 완료하였습니다.')
          } else {
            alert('글작성을 완료하지 못하였습니다..')
          }
        })
      this.submitted = true
    },
    newArticle() {
      this.cname =  '';
      this.subject = '';
      this.content = '';
      this.submitted = false;
      this.$router.push("/question/add")
    },
    goList(){
        this.$router.push("/question")
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
  }
}
</script>

<style>
.submitform {
  max-width: 300px;
  margin: auto;
}
</style>
