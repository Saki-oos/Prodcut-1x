<template>
  <v-container class="form-bg" fluid>
    <div class="bg-shapes"></div>
    <v-row align="center" justify="center" style="min-height: 100vh; position: relative; z-index: 2;">
      <v-col cols="12" sm="8" md="6" lg="5">
        <v-card elevation="14" class="pa-10 colorful-card">
          <v-card-title class="justify-center">
            <h2 class="form-title">สมัครสมาชิก</h2>
          </v-card-title>
          <v-card-text>
            <v-form v-model="valid" ref="form">
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
                v-model="password"
                :rules="[
                  v => !!v || 'กรุณากรอกรหัสผ่าน',
                  v => (v && v.length >= 8) || 'รหัสผ่านต้องมีอย่างน้อย 8 ตัวอักษร',
                  v => /[0-9]/.test(v) || 'ต้องมีตัวเลขอย่างน้อย 1 ตัว',
                  v => /[A-Z]/.test(v) || 'ต้องมีตัวพิมพ์ใหญ่อย่างน้อย 1 ตัว',
                  v => /[^a-zA-Z0-9]/.test(v) || 'ต้องมีอักขระพิเศษอย่างน้อย 1 ตัว',
                ]"
                label="รหัสผ่าน"
                type="password"
                outlined
                dense
                class="mb-4"
                color="pink"
                prepend-inner-icon="mdi-lock"
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

              <v-radio-group
                v-model="gender"
                :rules="[v => !!v || 'กรุณาเลือกเพศ']"
                label="เพศ"
                row
                class="mb-4"
                required
              >
                <v-radio label="ชาย" value="male" color="blue"></v-radio>
                <v-radio label="หญิง" value="female" color="pink"></v-radio>
                <v-radio label="ไม่ระบุ" value="not_specified" color="purple"></v-radio>
              </v-radio-group>

              <v-select
                v-model="selectedInterests"
                :items="interests"
                label="ความสนใจ"
                multiple
                chips
                :rules="[v => (v && v.length > 0) || 'กรุณาเลือกความสนใจอย่างน้อย 1 อย่าง']"
                outlined
                dense
                class="mb-4"
                color="teal"
                prepend-inner-icon="mdi-star"
                required
              ></v-select>

              <p v-if="selectedInterests && selectedInterests.length > 0" class="mb-4">
                <span class="font-weight-medium">ความสนใจของคุณ:</span>
                <span class="interest-list">{{ selectedInterests.join(', ') }}</span>
              </p>

              <v-row>
                <v-col cols="6">
                  <v-btn color="deep-purple accent-4" block large dark elevation="2" @click="submit">
                    <v-icon left>mdi-send</v-icon> ส่งข้อมูล
                  </v-btn>
                </v-col>
                <v-col cols="6">
                  <v-btn color="pink lighten-3" block large outlined elevation="2" @click="reset">
                    <v-icon left color="pink">mdi-refresh</v-icon> ล้างข้อมูล
                  </v-btn>
                </v-col>
              </v-row>
            </v-form>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      valid: false,
      name: '',
      email: '',
      password: '',
      confirmPassword: '',
      gender: null,
      selectedInterests: [],
      interests: [
        'อ่านหนังสือ',
        'ดูหนัง/ซีรีส์',
        'เล่นกีฬา',
        'ฟังเพลง',
        'เดินทาง/ท่องเที่ยว',
        'ทำอาหาร',
        'เล่นเกม',
        'เขียนโค้ด',
        'ถ่ายภาพ',
        'วาดรูป'
      ]
    };
  },
  methods: {
    submit() {
      if (this.$refs.form.validate()) {
        alert(`ส่งข้อมูลแล้ว: ${this.name}, ${this.email}`);
      }
    },
    reset() {
      this.$refs.form.reset();
      this.password = '';
      this.confirmPassword = '';
      this.gender = null;
      this.selectedInterests = [];
    },
  },
}
</script>

<style scoped>



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
  width: 420px; height: 420px;
  left: -120px; top: 60px;
  background: radial-gradient(circle, #fff 0%, #7c4dff 80%, transparent 100%);
  animation-delay: 0s;
}
.bg-shapes::after {
  width: 320px; height: 320px;
  right: -100px; bottom: 40px;
  background: radial-gradient(circle, #fff 0%, #00e5ff 80%, transparent 100%);
  animation-delay: 2s;
}
@keyframes floatShape {
  0% { transform: translateY(0) scale(1); }
  100% { transform: translateY(40px) scale(1.08); }
}
.form-title {
  font-weight: bold;
  color: #ffffff;
  letter-spacing: 1px;
}
.colorful-card {
  border-radius: 22px;
  background: linear-gradient(120deg,  #000000 100%);
  box-shadow: 0 8px 32px 0 rgba(124, 77, 255, 0.12), 0 1.5px 8px 0 #ff80ab33;
  position: relative;
  z-index: 2;
}
.v-btn {
  font-size: 1.1rem;
  font-weight: 500;
  border-radius: 12px;
  transition: background 0.2s;
}
.v-btn.deep-purple {
  color: #fff !important;
}
.v-btn.pink {
  color: #ad1457 !important;
}
.v-text-field,
.v-select {
  border-radius: 10px;
}
.v-radio .v-input--selection-controls__ripple {
  background: linear-gradient(90deg, #7c4dff 0%, #ff80ab 100%);
}
.interest-list {
  color: #00bfae;
  font-weight: bold;
  letter-spacing: 0.5px;
}
</style>