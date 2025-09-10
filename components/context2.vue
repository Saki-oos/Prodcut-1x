<template>
  <v-container class="contact-bg mini-bookstore-bg" fluid>
    <v-row align="center" justify="center" style="min-height: 100vh;">
      <v-col cols="12" sm="8" md="6" lg="5">
        <v-card elevation="6" class="pa-8 contact-card mini-card">
          <v-card-title class="justify-center contact-title mb-4 mini-title">
            ติดต่อเรา MiniBook
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
                  <v-btn color="deep-purple accent-2" block large class="mini-btn" @click="submit">
                    <v-icon left>mdi-send</v-icon> ส่งข้อความ
                  </v-btn>
                </v-col>
              </v-row>
            </v-form>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <v-snackbar
      v-model="snackbar"
      :timeout="5000"
      color="success"
      centered
      elevation="24"
      style="text-align:center; font-size:1.1rem;"
    >
      {{ snackbarMsg }}
    </v-snackbar>
  </v-container>
</template>

<script>
import axios from 'axios';
export default {
  name: 'ContactPage',
  data() {
    return {
      name: '',
      email: '',
      message: '',
      snackbar: false,
      snackbarMsg: '',
    };
  },
  methods: {
    async submit() {
      if (this.name && this.email && this.message) {
        try {
          const res = await axios.post('http://localhost/library_pytdb/uploads/contact_api.php', {
            name: this.name,
            email: this.email,
            message: this.message
          });
          if (res.data?.status === 'success') {
            this.snackbarMsg = 'ส่งข้อความเรียบร้อยแล้ว ขอบคุณที่ติดต่อเรา!';
            this.snackbar = true;
            this.name = '';
            this.email = '';
            this.message = '';
          } else {
            this.snackbarMsg = res.data?.message || 'เกิดข้อผิดพลาดในการส่งข้อมูล';
            this.snackbar = true;
          }
        } catch (err) {
          this.snackbarMsg = 'เกิดข้อผิดพลาดในการเชื่อมต่อ API';
          this.snackbar = true;
        }
      } else {
        this.snackbarMsg = 'กรุณากรอกข้อมูลให้ครบถ้วน';
        this.snackbar = true;
      }
    },
  },
};
</script>

<style scoped>
.contact-bg.mini-bookstore-bg {
  background: linear-gradient(120deg, #a2ccf7 60%, #0097fc 100%);
  min-height: 100vh;
  color: #111 !important;
  animation: none;
}
.mini-card {
  border-radius: 18px;
  background: linear-gradient(120deg, #6eb1f0 60%, #005a96 100%);
  box-shadow: 0 4px 24px 0 #bdbdbd33;
  color: #111 !important;
}
.mini-title {
  color: #7c3aed;
  font-weight: bold;
  letter-spacing: 1px;
  text-align: center;
  margin-bottom: 2rem;
}
.mini-btn {
  border-radius: 20px;
  font-weight: bold;
  letter-spacing: 1px;
  background: linear-gradient(90deg, #2166fa 60%, #008fee 100%);
  color: #fff !important;
}
.contact-email {
  color: #f472b6;
  font-weight: 600;
}
.contact-phone {
  color: #00bfae;
  font-weight: 600;
  margin-left: 4px;
}
.v-btn, .v-btn *, .v-icon, .v-label, .v-input__slot, .v-list-item__title, .v-chip, .v-select__selections, .v-text-field, .v-textarea, .v-select, .v-textarea *, .v-text-field *, .mini-title, .contact-title, p, span, label, h2, h1, h3, h4, h5, h6 {
  color: #111 !important;
}
</style>