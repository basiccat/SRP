<template>
    <div>
      <div class="login-wrap" v-show="showLogin">
        <h3>登录</h3>
        <input type="text" placeholder="请输入用户名" v-model="username">
        <input type="password" placeholder="请输入密码" v-model="password">
        <button v-on:click="login">登录</button>
        <span v-on:click="ToRegister">没有账号？马上注册</span>
      </div>

      <div class="register-wrap" v-show="showRegister">
        <h3>注册</h3>
        <input type="text" placeholder="请输入用户名" v-model="newUsername">
        <input type="password" placeholder="请输入密码" v-model="newPassword">
        <input type="role" placeholder="请输入身份" v-model="role">
        <input type="phone" placeholder="请输入电话号码" v-model="phone">
        <input type="room" placeholder="请输入房间号码" v-model="room_number">
        <input type="building" placeholder="请输入住所地址"v-model="building_number">
        <button v-on:click="register">注册</button>
        <span v-on:click="ToLogin">已有账号？马上登录</span>
      </div>
    </div>
</template>

<script>
    export default {
        data(){
          return{
            showLogin: true,
            showRegister: false,
            id: '',
            errCode: '',
            msg: '',
            phone: '',
            username: 'admin1',
            password: '123456',
            role: '',
            room_number: '',
            building_number: '',
            newUsername: '',
            newPassword: ''
          }
        },
        methods: {
          login(){
            if(this.username == ""||this.password == ""){
              alert("请正确输入用户名或密码")
            }else{
              let data = {'username':this.username,'password':this.password}
              this.$http.post('/user/login',data).then((res)=>{
                console.log(res)
                if(res.data.errCode !=0){
                  alert("登录信息出错")
                }else {
                  alert("登录成功")
                }
              })
            }
          },
          register(){
            if(this.newUsername == ""||this.newPassword==""||this.role==""||this.phone==""||this.room_number==""||this.building_number ==""){
              alert("请输入注册信息")
            }else{
              let data ={'username': this.newUsername,'password':this.newPassword,'role':this.role,'phone':this.phone,'room_number': this.room_number,'building_number':this.building_number}
              this.$http.post('/user/signUp',data).then((res) =>{
                console.log(res)
                if(res.data.errCode != 0){
                  alert("注册出现错误")
                }else {
                  this.username= ''
                  this.password= ''
                  this.phone=''
                  this.role=''
                  this.room_number = ''
                  this.building_number =''
                  setTimeout(function () {
                    this.showLogin = true
                    this.showRegister = false
                  }.bind(this),888)
                }
              })
            }
          },
          ToRegister(){
            this.showRegister = true
            this.showLogin = false
          },
          ToLogin(){
            this.showLogin = true;
            this.showRegister = false
          }

        }
    }
</script>

<style scoped>
  .login-wrap{text-align:center;}
  input{display:block; width:250px; height:40px; line-height:40px; margin:0 auto; margin-bottom: 10px; outline:none; border:1px solid #888; padding:10px; box-sizing:border-box;}
  p{color:red;}
  button{display:block; width:250px; height:40px; line-height: 40px; margin:0 auto; border:none; background-color:#41b883; color:#fff; font-size:16px; margin-bottom:5px;}
  span{cursor:pointer;}
  span:hover{color:#41b883;}
</style>
