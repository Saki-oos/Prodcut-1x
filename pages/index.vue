<template>
  <v-container class="form-bg" fluid>
    <v-row align="center" justify="center" style="min-height: 100vh;">
      <v-col cols="12" sm="8" md="6" lg="5">
        <v-card elevation="12" class="pa-8 colorful-card">
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
.form-bg {
  background: linear-gradient(135deg, #0087fdfa 0%, #ff02e6 100%);
  min-height: 100vh;
}
.form-title {
  font-weight: bold;
  color: #000000;
  letter-spacing: 1px;
 
}
.colorful-card {
  border-radius: 22px;
  background: linear-gradient(120deg, #ff00d0 0%, #1a4bed 100%);
  box-shadow: 0 8px 32px 0 rgba(124, 77, 255, 0.12), 0 1.5px 8px 0 #ff80ab33;
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