<template>
	<div class="login-container">
	  <h2>登录</h2>
	  <form @submit.prevent="handleLogin">
		<div class="form-group">
		  <label for="username">用户名:</label>
		  <input type="text" id="username" v-model="username" required />
		</div>
		<div class="form-group">
		  <label for="password">密码:</label>
		  <input type="password" id="password" v-model="password" required />
		</div>
		<button type="submit">登录</button>
	  </form>
	</div>
  </template>
  
  <script>
  import { ref } from 'vue';
  import axios from 'axios';
  
  export default {
	name: 'Login',
	setup() {
	  const username = ref('');
	  const password = ref('');
	  let token = '';
  
	  const handleLogin = async () => {
		try {
		  const response = await axios.post('http://localhost:4001/diary-server/login', {
			account: username.value,
			password: password.value,
		  });
		  if (response.status === 200) {
			// 处理登录成功逻辑，例如保存 token 等
			token = response.data.message;
			localStorage.setItem('token', token);
			console.log('token:', localStorage.getItem('token'));
		  } else {
			console.error('登录失败:', response.data.message);
		  }
		} catch (error) {
		  console.error('登录请求出错:', error);
		}
	  };
  
	  return {
		username,
		password,
		handleLogin,
	  };
	},
  };
  </script>
  
  <style scoped>
  .login-container {
	max-width: 400px;
	margin: 0 auto;
	padding: 20px;
	border: 1px solid #ccc;
	border-radius: 5px;
	box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  }
  
  .form-group {
	margin-bottom: 15px;
  }
  
  label {
	display: block;
	margin-bottom: 5px;
  }
  
  input {
	width: 100%;
	padding: 8px;
	box-sizing: border-box;
  }
  
  button {
	width: 100%;
	padding: 10px;
	background-color: #42b983;
	color: white;
	border: none;
	border-radius: 5px;
	cursor: pointer;
  }
  
  button:hover {
	background-color: #369f6e;
  }
  </style>
  