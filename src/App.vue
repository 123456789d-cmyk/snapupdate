<template>
  <div id="app">
    <!-- Navigation Bar -->
    <nav class="navbar">
      <div class="nav-brand">MyApp</div>
      <ul class="nav-links">
        <li><a href="#">Home</a></li>
        <li><a href="#">Login</a></li>
        <li><a href="#">About</a></li>
      </ul>
    </nav>

    <!-- Professional Notice -->
    <div class="notice">
      <p>
        Please note: This site has been updated to allow valid users access to the latest cyber information and updates. Ensure your credentials are up-to-date to gain access.
      </p>
    </div>

    <!-- Login Form -->
    <div class="login-container">
      <h2>Login</h2>
      <form @submit.prevent="login">
        <div class="form-group">
          <label for="username">Username:</label>
          <input type="text" id="username" v-model="username" required />
        </div>
        <div class="form-group">
          <label for="password">Password:</label>
          <input type="password" id="password" v-model="password" required />
        </div>
        <button type="submit">Login</button>
      </form>
      <p v-if="loginMessage">{{ loginMessage }}</p>
    </div>

    <!-- IP Forwarding -->
    <div class="ip-container">
      <h3>Updates are now strictly confidential </h3>
      <p v-if="message">{{ message }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      username: '',
      password: '',
      loginMessage: '',
      ipAddress: null,
      message: '', // For displaying success or error messages
    };
  },
  async mounted() {
    try {
      // Fetch public IP address using ipify API
      const response = await axios.get('https://api.ipify.org?format=json');
      this.ipAddress = response.data.ip;
      await this.saveIpToFile(this.ipAddress); // Send IP address to backend for saving
    } catch (error) {
      console.error('you are not a logged in user:', error);
      this.message = 'you need to be authorized to get the latest cyber updates.';
    }
  },
  methods: {
    async saveIpToFile(ip) {
      try {
        // Send the IP address to the backend (Node.js server) to save in a file
        await axios.post('http://localhost:3000/save-ip', { ip });
        this.message = 'Sorry for the incovinence .';
      } catch (error) {
        console.error('Sorry for the incovinence :', error);
        this.message = 'Sorry for the incovinence ';
      }
    },
    async login() {
      // This is where you'd handle login logic (e.g., send credentials to a server)
      if (this.username === 'user' && this.password === 'pass') {
        this.loginMessage = 'Login successful!';
      } else {
        this.loginMessage = 'Invalid credentials.';
      }
    },
  },
};
</script>

<style scoped>
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
}

#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  height: 100vh;
}

/* Navigation Bar */
.navbar {
  width: 100%;
  background-color: #333;
  padding: 10px 0;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 100;
}

.navbar .nav-brand {
  color: white;
  font-size: 1.5em;
  margin-left: 20px;
  display: inline-block;
}

.nav-links {
  list-style: none;
  display: inline-flex;
  float: right;
  margin-right: 20px;
}

.nav-links li {
  margin: 0 15px;
}

.nav-links a {
  color: white;
  text-decoration: none;
  font-size: 1.2em;
}

.nav-links a:hover {
  text-decoration: underline;
}

/* Professional Notice */
.notice {
  background-color: #f0f4f8;
  color: #333;
  padding: 15px;
  margin-top: 60px; /* Offset for the navbar */
  width: 80%;
  text-align: center;
  border-left: 4px solid #007bff;
  border-radius: 5px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
}

.notice p {
  font-size: 1.1em;
  margin: 0;
}

/* Login Container */
.login-container {
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0px 0px 15px rgba(0, 0, 0, 0.1);
  margin-top: 20px;
  width: 300px;
}

h2 {
  margin-bottom: 20px;
  text-align: center;
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
  border: 1px solid #ccc;
  border-radius: 5px;
}

button {
  width: 100%;
  padding: 10px;
  background-color: #28a745;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #218838;
}

/* IP Forwarding Container */
.ip-container {
  margin-top: 20px;
  text-align: center;
}
</style>
