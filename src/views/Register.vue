<template>
  <div class="home">
    <div class="background">
        <div class="shape"></div>
        <div class="shape"></div>
    </div>
    <form @submit.prevent="onRegister">
        <h3>Register Here</h3>

        <label for="username">Full Name</label>
        <input type="text" v-model="name" placeholder="Full Name" id="username">

        <label for="username">Email</label>
        <input type="email" v-model="email" placeholder="Email" id="username">

        <label for="password">Password</label>
        <input type="password" v-model="password" placeholder="Password" id="password">

        <button >Register</button>
        <div class="social">
          <router-link to="/" class="text-primary">Login instead</router-link>
        </div>
    </form>


  </div>
</template>

<script>
import { ref } from '@vue/reactivity';
import { useRouter } from "vue-router"
// @ is an alias to /src

export default {
  name: 'Home',
  components: {
    
  },

  setup() {
      const router = useRouter();

      const email = ref('')
      const password = ref('')
      const name = ref('')
      const onRegister = () => {
          const id = Math.floor(Math.random() * 1000);
          const users = JSON.parse(localStorage.getItem('users'));
          if (!users) {
              localStorage.setItem('users', JSON.stringify([
                {
                    id,
                  email: email.value,
                  password: password.value,
                  name: name.value,
                  verificationStatus1: false,
                  verificationStatus2: false,

                }
              ]))
          } else {
              
              users.push({
                  id,
                  email: email.value,
                  password: password.value,
                  name: name.value,
                  verificationStatus1: false,
                  verificationStatus2: false,

              })
              localStorage.setItem('users', JSON.stringify(users));
          }
          
          localStorage.setItem('token', id);
        router.push('/dashboard')
          email.value = ""
          name.value = ""
          password.value = ""
      }

      return {
        email,
        password,
        name,
        onRegister,
      }
  }
}
</script>

<style media="screen" scoped>
      *,
*:before,
*:after{
    padding: 0;
    margin: 0;
    box-sizing: border-box;
}
body{
    background-color: #080710;
}

.background{
    width: 430px;
    height: 520px;
    position: absolute;
    transform: translate(-50%,-50%);
    left: 50%;
    top: 50%;
}
.background .shape{
    height: 200px;
    width: 200px;
    position: absolute;
    border-radius: 50%;
}
.shape:first-child{
    background: linear-gradient(
        #1845ad,
        #23a2f6
    );
    left: -80px;
    top: -80px;
}
.shape:last-child{
    background: linear-gradient(
        to right,
        #ff512f,
        #f09819
    );
    right: -30px;
    bottom: -80px;
}
form{
    height: 520px;
    width: 400px;
    background-color: rgba(255,255,255,0.13);
    position: absolute;
    transform: translate(-50%,-50%);
    top: 50%;
    left: 50%;
    border-radius: 10px;
    backdrop-filter: blur(10px);
    border: 2px solid rgba(255,255,255,0.1);
    box-shadow: 0 0 40px rgba(8,7,16,0.6);
    padding: 50px 35px;
}
form *{
    font-family: 'Poppins',sans-serif;
    color: #ffffff;
    letter-spacing: 0.5px;
    outline: none;
    border: none;
}
form h3{
    font-size: 32px;
    font-weight: 500;
    line-height: 42px;
    text-align: center;
}

label{
    display: block;
    margin-top: 30px;
    font-size: 16px;
    font-weight: 500;
}
input{
    display: block;
    height: 50px;
    width: 100%;
    background-color: rgba(255,255,255,0.07);
    border-radius: 3px;
    padding: 0 10px;
    margin-top: 8px;
    font-size: 14px;
    font-weight: 300;
}
::placeholder{
    color: #e5e5e5;
}
button{
    margin-top: 50px;
    width: 100%;
    background-color: #ffffff;
    color: #080710;
    padding: 15px 0;
    font-size: 18px;
    font-weight: 600;
    border-radius: 5px;
    cursor: pointer;
}
.social{
  margin-top: 30px;
  display: flex;
}
.social div{
  background: red;
  width: 150px;
  border-radius: 3px;
  padding: 5px 10px 10px 5px;
  background-color: rgba(255,255,255,0.27);
  color: #eaf0fb;
  text-align: center;
}
.social div:hover{
  background-color: rgba(255,255,255,0.47);
}
.social .fb{
  margin-left: 25px;
}
.social i{
  margin-right: 4px;
}

    </style>