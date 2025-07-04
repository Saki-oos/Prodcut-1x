<template>
  <v-container class="contact-bg" fluid>
    <v-row align="center" justify="center" style="min-height: 100vh;">
      <v-col cols="12" sm="8" md="6" lg="5">
        <v-card elevation="6" class="pa-8 contact-card">
          <v-card-title class="justify-center contact-title mb-4">
            ติดต่อเรา
          </v-card-title>
          <v-card-text>
            <div class="text-center mb-6 grey--text">
              หากคุณมีคำถามหรือข้อเสนอแนะ กรุณากรอกแบบฟอร์มด้านล่าง หรือส่งอีเมลมาที่
              <span class="contact-email">krittanai2548@gmail.com</span>
              <br>
              <v-icon left color="primary" small>mdi-phone</v-icon>
              <span class="contact-phone">0960317654</span>
            </div>
            <v-form ref="form">
              <v-text-field
                v-model="name"
                label="ชื่อของคุณ"
                outlined
                dense
                class="mb-4"
                prepend-inner-icon="mdi-account"
                required
              ></v-text-field>
              <v-text-field
                v-model="email"
                label="อีเมล"
                outlined
                dense
                class="mb-4"
                prepend-inner-icon="mdi-email"
                required
              ></v-text-field>
              <v-textarea
                v-model="message"
                label="ข้อความ"
                outlined
                rows="4"
                class="mb-4"
                prepend-inner-icon="mdi-message-text"
                required
              ></v-textarea>
              <v-row>
                <v-col cols="12">
                  <v-btn color="primary" block large @click="submit">
                    <v-icon left>mdi-send</v-icon> ส่งข้อความ
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
  name: 'ContactPage',
  data() {
    return {
      name: '',
      email: '',
      message: '',
    };
  },
  methods: {
    submit() {
      if (this.name && this.email && this.message) {
        alert('ส่งข้อความเรียบร้อยแล้ว ขอบคุณที่ติดต่อเรา!');
        this.name = '';
        this.email = '';
        this.message = '';
      } else {
        alert('กรุณากรอกข้อมูลให้ครบถ้วน');
      }
    },
  },
};
</script>

<style scoped>

.contact-bg {
  background: linear-gradient(270deg, #dc00fe, #003cff, #ff00ea, #ff0073, #1100ff);
  background-size: 200% 200%;

}
.contact-card {
  border-radius: 18px;
  background: #000000;
  box-shadow: 0 4px 24px 0 rgba(80, 80, 160, 0.10);
}
.contact-title {
  font-size: 2rem;
  font-weight: bold;
  color: #1976d2;
  letter-spacing: 1px;
}
.contact-email {
  color: #7c4dff;
  font-weight: 500;
}
.contact-phone {
  color: #00e676;
  font-weight: 500;
  margin-left: 4px;
}
.v-btn {
  font-size: 1.1rem;
  font-weight: 500;
  border-radius: 10px;
}
</style>
