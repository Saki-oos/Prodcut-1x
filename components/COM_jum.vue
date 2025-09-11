<template>
  <v-container class="mini-bookstore-bg py-8" fluid>
    <v-row align="center" justify="center" style="min-height: 100vh;">
      <v-col cols="12" sm="8" md="6" lg="5">
        <v-card elevation="14" class="pa-10 colorful-card">
          <v-card-title class="justify-center">
            <h2 class="form-title mini-title">
              {{ isLoginMode ? 'เข้าสู่ระบบ MiniBook' : 'สมัครสมาชิก MiniBook' }}
            </h2>
          </v-card-title>
          <v-card-text>
            <v-form v-model="valid" ref="form">
              <v-text-field
                v-if="isLoginMode"
                v-model="identity"
                :rules="[v => !!v || 'กรุณากรอกชื่อหรืออีเมล']"
                label="ชื่อหรืออีเมล"
                outlined
                dense
                class="mb-4"
                color="indigo"
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
                color="pink"
                prepend-inner-icon="mdi-lock"
                required
              ></v-text-field>
              <template v-if="!isLoginMode">
                <v-text-field
                  v-model="name"
                  :rules="[v => !!v || 'กรุณากรอกชื่อ']"
                  label="ชื่อ"
                  outlined
                  dense
                  class="mb-4"
                  color="deep-purple accent-4"
                  prepend-inner-icon="mdi-account"
                  required
                ></v-text-field>
                <v-text-field
                  v-model="email"
                  :rules="[v => /.+@.+/.test(v) || 'อีเมลไม่ถูกต้อง']"
                  label="อีเมล"
                  outlined
                  dense
                  class="mb-4"
                  color="indigo"
                  prepend-inner-icon="mdi-email"
                  required
                ></v-text-field>
                <v-text-field
                  v-model="confirmPassword"
                  :rules="[v => v === password || 'รหัสผ่านไม่ตรงกัน']"
                  label="ยืนยันรหัสผ่าน"
                  type="password"
                  outlined
                  dense
                  class="mb-4"
                  color="pink"
                  prepend-inner-icon="mdi-lock-check"
                  required
                ></v-text-field>
              </template>
              <v-row>
                <v-col cols="12" class="mb-2">
                  <v-btn color="deep-purple accent-4" block large dark elevation="2" @click="submit">
                    <v-icon left>mdi-send</v-icon>
                    {{ isLoginMode ? 'เข้าสู่ระบบ' : 'สมัครสมาชิก' }}
                  </v-btn>
                </v-col>
                <v-col cols="12" class="mb-2">
                  <v-btn color="pink lighten-3" block large outlined elevation="2" @click="reset">
                    <v-icon left color="pink">mdi-refresh</v-icon> ล้างข้อมูล
                  </v-btn>
                </v-col>
                <v-col cols="12" class="mb-2">
                  <v-btn block text @click="toggleMode">
                    <v-icon left>mdi-account-switch</v-icon>
                    {{ isLoginMode ? 'สมัครสมาชิกใหม่' : 'เข้าสู่ระบบ' }}
                  </v-btn>
                </v-col>
                <v-col cols="12" v-if="isLoggedIn">
                  <v-btn color="red" block large elevation="2" @click="logout">
                    <v-icon left>mdi-logout</v-icon> ออกจากระบบ
                  </v-btn>
                </v-col>
              </v-row>
            </v-form>
            <div v-if="isLoggedIn" class="mt-6 text-center">
              <v-chip color="success" dark>
                เข้าสู่ระบบแล้ว: {{ user.name }} ({{ user.email }}) [{{ user.role }}]
              </v-chip>
            </div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <v-snackbar v-model="snackbar" :timeout="4000" color="success" centered>
      {{ snackbarMsg }}
    </v-snackbar>
  </v-container>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      valid: false,
      isLoginMode: true,
      isLoggedIn: false,
      user: null,
      identity: '', // สำหรับ login ด้วยชื่อหรืออีเมล
      password: '',
      name: '',
      email: '',
      confirmPassword: '',
      snackbar: false,
      snackbarMsg: '',
    };
  },
  methods: {
    async submit() {
      if (this.$refs.form.validate()) {
        if (this.isLoginMode) {
          // Login
          try {
            const res = await axios.post('http://localhost/library_pytdb/uploads/login_api.php', {
              identity: this.identity,
              password: this.password
            });
            if (res.data?.status === 'success') {
              this.snackbarMsg = 'เข้าสู่ระบบสำเร็จ!';
              this.snackbar = true;
              this.isLoggedIn = true;
              this.user = res.data.user;
              localStorage.setItem('token', res.data.token);
            } else {
              this.snackbarMsg = res.data?.message || 'เข้าสู่ระบบไม่สำเร็จ';
              this.snackbar = true;
            }
          } catch (err) {
            this.snackbarMsg = 'เกิดข้อผิดพลาดในการเชื่อมต่อ API';
            this.snackbar = true;
          }
        } else {
          // Register
          if (this.password !== this.confirmPassword) {
            this.snackbarMsg = 'รหัสผ่านไม่ตรงกัน';
            this.snackbar = true;
            return;
          }
          try {
            const res = await axios.post('http://localhost/library_pytdb/uploads/register_api.php', {
              name: this.name,
              email: this.email,
              password: this.password
            });
            if (res.data?.status === 'success') {
              this.snackbarMsg = 'สมัครสมาชิกสำเร็จ! กรุณาเข้าสู่ระบบ';
              this.snackbar = true;
              this.toggleMode();
              this.reset();
            } else {
              this.snackbarMsg = res.data?.message || 'สมัครสมาชิกไม่สำเร็จ';
              this.snackbar = true;
            }
          } catch (err) {
            this.snackbarMsg = 'เกิดข้อผิดพลาดในการเชื่อมต่อ API';
            this.snackbar = true;
          }
        }
      }
    },
    reset() {
      this.$refs.form.reset();
      this.identity = '';
      this.name = '';
      this.email = '';
      this.password = '';
      this.confirmPassword = '';
    },
    toggleMode() {
      this.isLoginMode = !this.isLoginMode;
      this.reset();
    },
    logout() {
      this.isLoggedIn = false;
      this.user = null;
      localStorage.removeItem('token');
      this.snackbarMsg = 'ออกจากระบบเรียบร้อย';
      this.snackbar = true;
    }
  }
}
</script>

<style scoped>
.mini-bookstore-bg {
  background: linear-gradient(120deg, #d3a4ff 60%, #f3c4e4 100%);
  min-height: 100vh;
  color: #111 !important;
}
.mini-title {
  color: #7c3aed;
  font-weight: bold;
  letter-spacing: 1px;
  text-align: center;
  margin-bottom: 2rem;
}
.colorful-card {
  border-radius: 22px;
  background: linear-gradient(120deg, #f19edd 60%, #fa9adc 100%);
  box-shadow: 0 8px 32px 0 rgba(124, 77, 255, 0.12), 0 1.5px 8px 0 #ff80ab33;
  position: relative;
  z-index: 2;
  color: #111 !important;
}
.v-btn {
  font-size: 1.1rem;
  font-weight: 500;
  border-radius: 12px;
  transition: background 0.2s;
}
.v-btn.deep-purple {
  color: #fff !important;
  background: linear-gradient(90deg, #a78bfa 60%, #f472b6 100%) !important;
}
.v-btn.pink {
  color: #ad1457 !important;
  background: #ffe3f6 !important;
}
.v-text-field,
.v-select {
  border-radius: 10px;
  background: #a760f8;
  color: #111 !important;
}
.v-label,
.v-input__slot,
.v-radio label,
.v-select__selections,
.v-chip,
.v-list-item__title {
  color: #111 !important;
}
.v-radio .v-input--selection-controls__ripple {
  background: linear-gradient(90deg, #7c4dff 0%, #ff80ab 100%);
}
.interest-list {
  color: #00bfae;
  font-weight: bold;
  letter-spacing: 0.5px;
}
.v-application, .v-application * {
  color: #111 !important;
}
</style>