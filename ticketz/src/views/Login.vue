<template>
    <form @submit.prevent>
        <label for="username">Username</label>
        <input v-model="username" type="text" placeholder="Username.." id="username">
        <label for="password">Password</label>
        <input v-model="password" type="password" placeholder="Username.." id="password">
        <button type="submit" @click="login" >Login</button>
        <label>{{ error_text }}</label>
    </form>
</template>

<style scoped>
body {
    background-color: #CFEBDF;
    font-family: Arial, sans-serif;
}

form {
    background-color: #E2FADB;
    border-radius: 10px;
    padding: 20px;
    margin: 50px auto;
    max-width: 400px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
}

h1 {
    text-align: center;
    color: #5F634F;
}

label {
    display: block;
    margin-bottom: 10px;
    color: #5F634F;
}

input[type="text"], input[type="password"] {
    display: block;
    width: 100%;
    padding: 10px;
    border-radius: 5px;
    border: none;
    background-color: #DBEFBC;
    margin-bottom: 20px;
}

button[type="submit"] {
    background-color: #9BC4CB;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
    transition: background-color 0.3s;
}

button[type="submit"]:hover {
    background-color: #5F634F;
}

p {
    color: red;
    margin-top: 20px;
    text-align: center;
    font-weight: bold;
}
</style>

<script>
import axios from 'axios'
import router from '../router/index.js'

export default {
	data() {
		return {
			URL: "http://127.0.0.1:8000/api-token-auth/",
			username: "",
            password: "",
            error_text: ""
		}
	},
	methods: {
		async login() {
			try {
                const response = await axios.post(this.URL, {
                    username: this.username,
                    password: this.password
                });
                localStorage.setItem('ticketz_token', response.data.token)
				router.push({name: "home"})
				this.tickers = response.data
			} catch (error) {
                if(error.response.status !== 400) {
                    this.error_text = error.response.statusText
                }
                else {
                    this.error_text = "Username or password incorrect"
                }
			}
		}
	}
}
</script>