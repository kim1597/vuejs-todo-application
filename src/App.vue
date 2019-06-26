<template>
<div>
  <h1>Vue Form 제작</h1>
  <h2>Writen by 김영환</h2>
  <form v-on:submit.prevent="submitForm">
    <!-- <ID Input box></ID> -->
    <div>
      <label for="username">id:</label>
      <input id="username" type="text" v-model="username">
    </div>
    <div>
      <label for="password">password:</label>
      <input id="password" type="password" v-model="password">
    </div>
    <!-- <로그인 버튼 영역> -->
    <button type="submit" v-bind:disabled="isUsernameValid">Login</button>
  </form>
  <!-- <p class="log-message" v-if="isUsernameValid">ID형식이 맞습니다.</p></p> -->
  <!-- <p class="log-message">
    <template v-if="isUsernameValid">
      ID형식이 맞습니다.
    </template>
    <template v-else>
      ID형식이 맞지 않습니다.
    </template>
  </p> -->
  <p class="log-message" v-if="!isUsernameValid">ID형식이 맞지 않습니다.</p></p>
</div>

</template>

<script>
function checkEmailFormat(email) {
  var re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
  return re.test(String(email).toLowerCase());
}

export default {
  data() {
    return {
      username: "",
      password: "",
      logMessage:"",
    }
  },
  computed: {
    isFormInvalid: function() {
      // if (
      //   this.username === '' && 
      //   this.password === '' &&
      //  !this.isUsernameValid
      // ) {
      //   return ture;
      // } else {
      //   return flase;
      // }
      return (
        !this.username || ! this.password || checkEmailFormat
      )
    },
    isUsernameValid: function() {
      if (this.username === "") {
        return true;
      }
      // username이 이메일인지 확인, 맞으면 true, 틀리면 false
      return checkEmailFormat(this.username)
      // if (this.username === '이메일')
      // return this.data 
    }
  },
  methods: {
    submitForm: function(event) {
      // debugger;
      event.preventDefault();
      console.log('제출', event);

      $.ajax({
          url: 'https://jsonplaceholder.typicode.com/users/',
          method: 'POST',
          dataType: 'json',
          contentType: "application/json; charset=UTF-8",
          data: JSON.stringify({
            name: this.username,
            password: this.password,
          })
        })
        .then(function(response) {
          console.log(response);
        })
        .catch(function(error) {
          console.log(error);
        });
    },
  },
}
</script>

<style scoped>
.log-message {
  color: red;
}
</style>
