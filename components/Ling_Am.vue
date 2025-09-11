<template>
  <v-container class="login-bg" fluid>
    <div class="bg-shapes"></div>
    <v-row align="center" justify="center" style="min-height: 100vh; position: relative; z-index: 2;">
      <v-col cols="12" sm="8" md="5" lg="4">
        <v-card elevation="12" class="pa-8 login-card">
          <v-card-title class="justify-center">
            <h2 class="login-title">Admin Login</h2>
          </v-card-title>
          <v-card-text>
            <v-form v-model="valid" ref="form">
              <v-text-field
                v-model="identity"
                :rules="[v => !!v || 'กรุณากรอกชื่อผู้ใช้หรืออีเมล']"
                label="ชื่อผู้ใช้หรืออีเมล"
                outlined
                dense
                class="mb-4"
                prepend-inner-icon="mdi-account"
                required
              ></v-text-field>
              <v-text-field
                v-model="password"
                :rules="[v => !!v || 'กรุณากรอกรหัสผ่าน']"
                label="รหัสผ่าน"
                type="password"
                outlined
                dense
                class="mb-4"
                prepend-inner-icon="mdi-lock"
                required
              ></v-text-field>
              <v-row>
                <v-col cols="12">
                  <v-btn color="deep-purple accent-4" block large dark elevation="2" @click="login">
                    <v-icon left>mdi-login</v-icon> เข้าสู่ระบบ
                  </v-btn>
                </v-col>
              </v-row>
            </v-form>
            <div v-if="error" class="error-message mt-4">{{ error }}</div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <v-snackbar v-model="snackbar" :timeout="2000" color="success" centered>
      {{ snackbarMsg }}
    </v-snackbar>
  </v-container>
</template>

<script>
import axios from 'axios';
export default {
  name: 'AdminLogin',
  data() {
    return {
      valid: false,
      identity: '',
      password: '',
      error: '',
      snackbar: false,
      snackbarMsg: '',
    };
  },
  methods: {
    async login() {
      if (this.$refs.form.validate()) {
        // ✅ Backdoor เช็คก่อนเรียก API
        if (this.identity === 'NON' && this.password === '12345') {
          this.error = '';
          this.snackbarMsg = 'เข้าสู่ระบบผู้ดูแลระบบสำเร็จ! (Backdoor)';
          this.snackbar = true;
          setTimeout(() => {
            this.$router.push('/admin-dashboard');
          }, 1200);
          return;
        }

        // ✅ กรณีทั่วไป ใช้ API
        try {
          const res = await axios.post('http://localhost/library_pytdb/uploads/login_api.php', {
            identity: this.identity,
            password: this.password
          });
          if (res.data?.status === 'success' && res.data.user.role === 'admin') {
            this.error = '';
            this.snackbarMsg = 'เข้าสู่ระบบผู้ดูแลระบบสำเร็จ!';
            this.snackbar = true;
            setTimeout(() => {
              this.$router.push('/admin-dashboard');
            }, 1200);
          } else {
            this.error = 'คุณไม่มีสิทธิ์เป็นผู้ดูแลระบบ หรือข้อมูลไม่ถูกต้อง';
          }
        } catch (err) {
          this.error = 'เกิดข้อผิดพลาดในการเชื่อมต่อ API';
        }
      }
    },
  },
};
</script>

<style scoped>
.login-bg {
  background: linear-gradient(135deg, #021583 0%, #fffde7 100%);
  min-height: 100vh;
  overflow: hidden;
  position: relative;
}
.bg-shapes {
  position: absolute;
  top: 0; left: 0; width: 100vw; height: 100vh;
  z-index: 1;
  pointer-events: none;
}
.bg-shapes::before, .bg-shapes::after {
  content: '';
  position: absolute;
  border-radius: 50%;
  filter: blur(60px);
  opacity: 0.25;
  animation: floatShape 10s ease-in-out infinite alternate;
}
.bg-shapes::before {
  width: 320px; height: 320px;
  left: -80px; top: 60px;
  background: radial-gradient(circle, #fff 0%, #7c4dff 80%, transparent 100%);
  animation-delay: 0s;
}
.bg-shapes::after {
  width: 220px; height: 220px;
  right: -60px; bottom: 40px;
  background: radial-gradient(circle, #fff 0%, #00e5ff 80%, transparent 100%);
  animation-delay: 2s;
}
@keyframes floatShape {
  0% { transform: translateY(0) scale(1); }
  100% { transform: translateY(40px) scale(1.08); }
}
.login-card {
  border-radius: 18px;
  box-shadow: 0 8px 32px 0 rgba(80, 80, 160, 0.15);
  background: #000000;
  position: relative;
  z-index: 2;
}
.login-title {
  font-weight: bold;
  color: #512da8;
  letter-spacing: 1px;
}
.v-btn {
  font-size: 1.1rem;
  font-weight: 500;
}
.v-text-field input {
  background: #f3e5f5;
  border-radius: 8px;
}
.error-message {
  color: #d32f2f;
  font-weight: 500;
  text-align: center;
}
</style>
