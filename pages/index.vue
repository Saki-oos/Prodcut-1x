<template>

  <v-container>

     <h1></h1>
    <v-form v-model="valid" ref="form">
      <v-text-field
        v-model="name"
        :rules="[v => !!v || 'กรุณากรอกชื่อ']"
        label="ชื่อ"
        required
      ></v-text-field>

      <v-text-field
        v-model="email"
        :rules="[v => /.+@.+/.test(v) || 'อีเมลไม่ถูกต้อง']"
        label="อีเมล"
        required
      ></v-text-field>

       <v-text-field

  v-model="password"
  :rules="[
    v => !!v || 'กรุณากรอกรหัสผ่าน',
    v => (v && v.length >= 8) || 'รหัสผ่านต้องมีอย่างน้อย 8 ตัวอักษร,ต้องมีตัวเลขอย่างน้อย 1 ตัว,ต้องมีตัวพิมพ์ใหญ่อย่างน้อย 1 ตัว,ต้องมีอักขระพิเศษอย่างน้อย 1 ตัว',
    // สามารถเพิ่มกฎอื่นๆ ได้ตามต้องการ เช่น ต้องมีตัวเลข, ตัวพิมพ์ใหญ่, อักขระพิเศษ
    // v => /[0-9]/.test(v) || 'ต้องมีตัวเลขอย่างน้อย 1 ตัว',
    // v => /[A-Z]/.test(v) || 'ต้องมีตัวพิมพ์ใหญ่อย่างน้อย 1 ตัว',
    // v => /[^a-zA-Z0-9]/.test(v) || 'ต้องมีอักขระพิเศษอย่างน้อย 1 ตัว',
  ]"
  label="รหัสผ่าน"
  type="password" required

></v-text-field>
      <v-text-field
        v-model="confirmPassword"
        :rules="[v => v === password || 'รหัสผ่านไม่ตรงกัน']"
        label="ยืนยันรหัสผ่าน"
        type="password"
        required
      ></v-text-field>

      <v-radio-group
        v-radio-group 
         :rules="[v => !!v || 'กรุณาเลือกเพศ']" 
        label="เพศ"
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
        multiple chips :rules="[v => (v && v.length > 0) || 'กรุณาเลือกความสนใจอย่างน้อย 1 อย่าง']"
        required
      ></v-select>

       <p v-if="selectedInterests && selectedInterests.length > 0">
        ความสนใจของคุณ: {{ selectedInterests.join(', ') }}
      </p>

      

      <v-btn color="primary" @click="submit">ส่งข้อมูล</v-btn>
      <v-btn text @click="reset">ล้างข้อมูล</v-btn>
    </v-form>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
     valid: false,
      name: '',
      email: '',
      gender: null,
      selectedInterests: [], // เปลี่ยนเป็น Array เปล่าสำหรับเก็บหลายค่า
      interests: [ // รายการตัวเลือกความสนใจ
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
    },
  },
}
</script>