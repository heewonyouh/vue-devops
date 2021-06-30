<template>
    <div>  
    <div>
      <table class="list_table">
        <col width="20%" />
        <col width="20%" />
        <col width="10%" />
        <tr>
          <th>제목</th>
          <th>작성자</th>
          <th>작성날짜</th>
        </tr>
        <tr  v-for="(article, index) in articles" :key="index" >
            <td>
          <router-link class="btn btn-primary" :to="`question/view/${article.idx}`">{{article.subject}}</router-link>
          </td>
          <td class="button">
            {{article.writer}}
          </td>
          <td class="button">
            {{article.createDate}}
          </td>
        </tr>
      </table>
    </div>
    <div class="search_box">
      이름으로 찾기 :
      <input type="text" name="name" v-model="cname" @keyup.enter="searchname" v-cloak />
    </div>
    <div style="text-align:right; padding-right:50px">
          <router-link style="text-align:right"  to="question/add">질문등록하기</router-link>    
    </div>    
      </div>
</template>

<script>
import http from "../http-common4";
export default {
    
  name: 'question',
  data() {
    return {
      articles:[],
      cname:""
    };
  },
  methods: {
    selectApt: function(apt) {
      this.$emit('select-apt', apt);
    },
    retrieveArticles() {
      http
        .get("/findAllArticle")
        .then(response => (this.articles = response.data))
        .catch(() => {
          this.errored = true;
        })
        .finally(() => (this.loading = false));
    },
    searchname(){
        http
        .get("/findArticleBySubject/"+this.cname)
        .then(response => (this.articles = response.data))
        .catch(() => {
          this.errored = true;
        })
        .finally(() => (this.loading = false));
    }
  },
  mounted() {
    this.retrieveArticles();
  }
};
</script>