<template>
  <Container class="signup-page">
    <div class="form-container">
      <h1>Join Hypeo</h1>
      <p>Sign up now to unlock real-time sentiment alerts and advanced features.</p>
      
      <div id="google-login-button" style="margin: 20px 0;"></div>
      
      <p style="margin-bottom: 20px;">--- OR sign up with Email ---</p>

      <form @submit.prevent="handleSignup">
        <input 
          v-model="email" 
          type="email" 
          placeholder="Enter your email" 
          required 
        />
        <input 
          v-model="password" 
          type="password" 
          placeholder="Create a password" 
          required 
        />
        <button type="submit" :disabled="isLoading">
          {{ isLoading ? 'Processing...' : 'Sign Up' }}
        </button>
      </form>

      <div v-if="error" class="error-message">{{ error }}</div>
      <div v-if="success" class="success-message">Registration successful! Redirecting...</div>

    </div>
  </Container>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import pageData from '../data/index.json'; // Path diperbaiki

const page = ref(pageData); 
const GOOGLE_CLIENT_ID = page.value.config.googleClientId;

const email = ref('');
const password = ref('');
const isLoading = ref(false);
const error = ref(null);
const success = ref(false);

function handleGoogleCredentialResponse(response) {
  console.log("Encoded JWT ID token: " + response.credential);
  alert('Google Login Successful! Token received.');
}

const initGoogleLogin = () => {
    if (typeof google !== 'undefined' && google.accounts && google.accounts.id) {
        google.accounts.id.initialize({
            client_id: GOOGLE_CLIENT_ID,
            callback: handleGoogleCredentialResponse
        });

        google.accounts.id.renderButton(
            document.getElementById("google-login-button"),
            { theme: "filled_blue", size: "large", text: "signup_with", width: "380" }
        );
    } else {
        setTimeout(initGoogleLogin, 100);
    }
};


const handleSignup = async () => {
  error.value = null;
  isLoading.value = true;
  
  try {
    await new Promise(resolve => setTimeout(resolve, 1500)); 
    
    success.value = true;
  } catch (err) {
    error.value = 'Failed to create account. Please try again.';
  }
  
  isLoading.value = false;
};


onMounted(() => {
    initGoogleLogin();
});
</script>

<style scoped>
.signup-page {
  padding: 50px 20px;
  min-height: 80vh;
}
.form-container {
  max-width: 400px;
  margin: 0 auto;
  text-align: center;
}
input, button {
  width: 100%;
  padding: 12px;
  margin: 8px 0;
  border: 1px solid #ccc;
  border-radius: 4px;
}
button {
  background-color: #007bff; 
  color: white;
  cursor: pointer;
  border: none;
}
.error-message {
  color: red;
  margin-top: 10px;
}
.success-message {
  color: green;
  margin-top: 10px;
}
</style>
