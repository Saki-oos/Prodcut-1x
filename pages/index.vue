<template>
  <v-container class="form-bg" fluid>
    <v-row align="center" justify="center" style="min-height: 100vh;">
      <v-col cols="12" sm="8" md="6" lg="5">
        <v-card elevation="10" class="pa-8">
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
                required
              ></v-text-field>

              <v-text-field
                v-model="email"
                :rules="[v => /.+@.+/.test(v) || 'อีเมลไม่ถูกต้อง']"
                label="อีเมล"
                outlined
                dense
                class="mb-4"
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
                <v-radio label="ชาย" value="male"></v-radio>
                <v-radio label="หญิง" value="female"></v-radio>
                <v-radio label="ไม่ระบุ" value="not_specified"></v-radio>
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
                required
              ></v-select>

              <p v-if="selectedInterests && selectedInterests.length > 0" class="mb-4">
                <span class="font-weight-medium">ความสนใจของคุณ:</span>
                <span class="deep-purple--text">{{ selectedInterests.join(', ') }}</span>
              </p>

              <v-row>
                <v-col cols="6">
                  <v-btn color="primary" block large @click="submit">ส่งข้อมูล</v-btn>
                </v-col>
                <v-col cols="6">
                  <v-btn color="grey lighten-1" block large outlined @click="reset">ล้างข้อมูล</v-btn>
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
  background: linear-gradient(135deg, #ede7f6 0%, #fffde7 100%);
  min-height: 100vh;
}
.form-title {
  font-weight: bold;
  color: #512da8;
  letter-spacing: 1px;
}
.v-card {
  border-radius: 18px;
  box-shadow: 0 8px 32px 0 rgba(80, 80, 160, 0.15);
}
.v-btn {
  font-size: 1.1rem;
  font-weight: 500;
}