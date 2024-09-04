<template>
  <div>
    <v-img
      class="mx-auto my-6"
      max-width="228"
      src="https://cdn.vuetifyjs.com/docs/images/logos/vuetify-logo-v3-slim-text-light.svg"
    ></v-img>

    <v-card class="mx-auto pa-12 pb-8" elevation="8" max-width="448" rounded="lg">
      <div class="text-subtitle-1 text-medium-emphasis">Account</div>

      <v-text-field
        v-model="email"
        density="compact"
        placeholder="Email address"
        prepend-inner-icon="mdi-email-outline"
        variant="outlined"
      ></v-text-field>

      <div class="text-subtitle-1 text-medium-emphasis d-flex align-center justify-space-between">
        Password
        <a class="text-caption text-decoration-none text-blue" href="#" @click.prevent="forgotPassword">
          Forgot login password?
        </a>
      </div>

      <v-text-field
        v-model="passwd"
        :append-inner-icon="visible ? 'mdi-eye-off' : 'mdi-eye'"
        :type="visible ? 'text' : 'password'"
        density="compact"
        placeholder="Enter your password"
        prepend-inner-icon="mdi-lock-outline"
        variant="outlined"
        @click:append-inner="visible = !visible"
      ></v-text-field>

      <v-card class="mb-12" color="surface-variant" variant="tonal">
        <v-card-text class="text-medium-emphasis text-caption">
          Warning: After 3 consecutive failed login attempts, your account will be temporarily locked for three hours. If you must login now, you can also click "Forgot login password?" below to reset the login password.
        </v-card-text>
      </v-card>

      <v-btn class="mb-8" color="blue" size="large" variant="tonal" block @click="doLogin">
        Log In
      </v-btn>

      <v-card-text class="text-center">
        <a class="text-blue text-decoration-none" href="#" @click.prevent="goToSignUp">
          Registration now <v-icon icon="mdi-chevron-right"></v-icon>
        </a>
      </v-card-text>
    </v-card>
  </div>
</template>

<script>
import axios from 'axios';
definePageMeta({
  layout: "#",
});

export default {
  data: () => ({
    visible: false,
    email: '',
    passwd: ''
  }),
  methods: {
    async doLogin() {
      let forms = {
        email: this.email,
        passwd: this.passwd
      };
      console.log(forms);
      try {
        const response = await axios.post('http://localhost:7000/login', forms);
        const data = response.data;
        console.log(data.status);
        if (data.status == 'success') {
          console.log(data.row.email);
          window.sessionStorage.setItem("token", JSON.stringify(data.token));
          this.$router.replace('/home');
        } else {
          // แจ้งเตือนกรณี login ไม่สำเร็จ
          alert(data.masages);
          this.$router.replace('/login');
        }
      } catch (error) {
        // แสดงข้อผิดพลาด
        console.error('Login failed', error);
        alert('Login failed: ' + error.message);
      }
    },
    forgotPassword() {
      // โค้ดสำหรับลืมรหัสผ่าน
      alert('Forgot password functionality will be implemented soon.');
    },
    goToSignUp() {
      this.$router.push('/signup');
    }
  },
};
</script>

<style scoped>
.fill-height {
  height: 100vh;
}
</style>
