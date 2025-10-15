<template>
  <Container class="login-page">
    <div class="form-container">
      <h1>Log In to Hypeo</h1>
      <p>Welcome back! Please enter your details to continue.</p>
      
      <div id="google-login-button" style="margin: 20px 0;"></div>
      
      <p style="margin-bottom: 20px;">--- OR log in with Email ---</p>

      <form @submit.prevent="handleLogin">
        <input 
          v-model="email" 
          type="email" 
          placeholder="Enter your email" 
          required 
        />
        <input 
          v-model="password" 
          type="password" 
          placeholder="Enter your password" 
          required 
        />
        <button type="submit" :disabled="isLoading">
          {{ isLoading ? 'Processing...' : 'Log In' }}
        </button>
      </form>

      <div v-if="error" class="error-message">{{ error }}</div>
      <p class="mt-4">
        Don't have an account? 
        <NuxtLink to="/signup" class="text-blue-500">Sign Up</NuxtLink>
      </p>
    </div>
  </Container>
</template>

<script setup>
import { ref, onMounted } from 'vue';
// Pastikan path ini benar relatif terhadap folder `pages`
import pageData from '../data/index.json'; 

const page = ref(pageData); 
// Client ID Anda akan diambil dari data/index.json
const GOOGLE_CLIENT_ID = page.value.config.googleClientId;

const email = ref('');
const password = ref('');
const isLoading = ref(false);
const error = ref(null);

function handleGoogleCredentialResponse(response) {
  console.log("Encoded JWT ID token: " + response.credential);
  alert('Google Login Successful! Token received.');
}

const initGoogleLogin = () => {
    if (typeof google !== 'undefined' && google.accounts && google.accounts.id) {
        google.accounts.id.initialize({
            client_id: GOOGLE_CLIENT_ID,
            callback: handleGoogleCredentialResponse,
            context: 'signin'
        });

        google.accounts.id.renderButton(
            document.getElementById("google-login-button"),
            { theme: "filled_blue", size: "large", text: "signin_with", width: "100%" }
        );
    } else {
        setTimeout(initGoogleLogin, 100);
    }
};

const handleLogin = async () => {
  // Logic Anda di sini
};

onMounted(() => {
    initGoogleLogin();
});
</script>

<style scoped>
.login-page {
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
.mt-4 {
  margin-top: 1rem;
}
.text-blue-500 {
  color: #3b82f6;
}
</style>

---

## 2. Verifikasi `components/Navbar.vue`

Pastikan kode yang Anda *push* untuk `Navbar.vue` sudah sesuai dengan perbaikan terakhir, yaitu menggunakan **`<NuxtLink>`** dan mengarah ke **`/login`**:

```vue
<script setup>
const menuitems = [
  // ...
  {
    title: "Log in",
    url: "/login", // Pastikan ini path internal
  },
];
</script>

<template>
    <NuxtLink :to="item.url" class="flex px-3 py-2 text-gray-600 border rounded-sm hover:text-gray-800">
        {{ item.title }}
    </NuxtLink>
    </template>
