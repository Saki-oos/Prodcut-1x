<template>
  <v-container class="welcome-bg mini-bookstore-bg" fluid>
    <!-- พื้นหลังไล่สีชมพู-ขาว สไตล์มินิมอล -->
    <div class="bg-shapes"></div>
    <v-row align="center" justify="center" style="min-height: 100vh; position: relative; z-index: 2;">
      <v-col cols="12" md="8" lg="6">
        <v-card elevation="12" class="pa-10 welcome-card">
          <v-row align="center" justify="center">
            <v-avatar size="100" class="mb-6 welcome-logo">
              <!-- โลโก้ร้าน: ไอคอนหนังสือสีม่วงสดใส -->
              <v-icon size="80" color="deep-purple accent-2">mdi-book-open-page-variant</v-icon>
            </v-avatar>
          </v-row>
          <v-card-title class="justify-center display-1 font-weight-bold welcome-title mb-4 mini-title">
            <!-- หัวข้อร้าน: สีชมพูสดใส ตัวหนา -->
            ยินดีต้อนรับสู่ร้านหนังสือ MiniBook
          </v-card-title>
          <v-card-text class="text-center mb-6">
            <div class="subtitle-1 mb-2">ระบบนี้ช่วยให้คุณสมัครสมาชิกได้อย่างรวดเร็วและปลอดภัย</div>
            <div class="mb-4">กรุณาคลิกปุ่มด้านล่างเพื่อเริ่มต้นการสมัครสมาชิก</div>
          </v-card-text>
          <v-row justify="center">
            <v-btn color="deep-purple accent-2" large class="welcome-btn mini-btn" to="/loing">
              <!-- ปุ่มสมัครสมาชิก: สีม่วง-ชมพูสดใส ขอบมน ตัวหนา -->
              สมัครสมาชิก
              <v-icon left>mdi-account-plus</v-icon>
            </v-btn>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'WelcomePage',
}
</script>

<style scoped>
.welcome-bg.mini-bookstore-bg {
  /* พื้นหลังไล่สีชมพู-ขาว เต็มจอ */
  background: linear-gradient(120deg, #fff6fa 0%, #ffe3f6 100%);
  min-height: 100vh;
  min-width: 100vw;
  width: 100vw;
  height: 100vh;
  position: fixed;
  top: 0;
  left: 0;
  color: #111 !important;
  animation: none;
  z-index: 0;
}
.mini-card, .welcome-card {
  /* กล่องเนื้อหา ขอบมน พื้นหลังขาว-ชมพู */
  border-radius: 32px;
  background: linear-gradient(120deg, #fff 60%, #ffe3f6 100%);
  box-shadow: 0 8px 32px 0 #f472b633, 0 1.5px 8px 0 #a78bfa33;
  color: #111 !important;
}
.mini-title, .welcome-title {
  /* หัวข้อสีชมพูสดใส ตัวหนา */
  color: #f472b6;
  font-weight: bold;
  letter-spacing: 1px;
  text-align: center;
  margin-bottom: 2rem;
  text-shadow: 0 2px 8px #fbbf2444;
}
.mini-btn, .welcome-btn {
  /* ปุ่มสีม่วง-ชมพูสดใส ขอบมน ตัวหนา */
  border-radius: 24px;
  font-weight: bold;
  letter-spacing: 1px;
  background: linear-gradient(90deg, #a78bfa 60%, #f472b6 100%);
  color: #fff !important;
  font-size: 1.25rem;
  box-shadow: 0 4px 16px 0 #f472b655, 0 2px 8px 0 #a78bfa33;
  padding: 12px 32px;
}
.welcome-logo {
  /* โลโก้ร้าน: วงกลมขาว ขอบชมพู เงา */
  background: #fff;
  border-radius: 50%;
  border: 5px solid #f472b6;
  box-shadow: 0 2px 12px #ffffffaa;
  display: flex;
  align-items: center;
  justify-content: center;
}
.welcome-title, .mini-title, .welcome-card, .mini-card, .welcome-btn, .mini-btn, .v-btn, .v-btn *, .v-icon, .v-label, .v-input__slot, .v-list-item__title, .v-chip, .v-select__selections, .v-text-field, .v-textarea, .v-select, .v-textarea *, .v-text-field *, p, span, label, h2, h1, h3, h4, h5, h6 {
  /* ตัวหนังสือสีดำ */
  color: #ffffff !important;
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
@keyframes gradientMove {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}
@keyframes floatShape {
  0% { transform: translateY(0) scale(1); }
  100% { transform: translateY(40px) scale(1.08); }
}
.welcome-card {
  border-radius: 28px;
  background: linear-gradient(120deg,  #000000 100%);
  box-shadow: 0 8px 32px 0 rgba(124, 77, 255, 0.12), 0 1.5px 8px 0 #80d8ff33;
}
.welcome-title {
  color: #0514e2;
  text-shadow: 1px 2px 8px #b388ff44;
}   
.welcome-btn {
  font-size: 1.2rem;
  font-weight: 600;
  border-radius: 16px;
  box-shadow: 0 2px 8px 0 #7c4dff33;
  letter-spacing: 1px; 
}
.v-avatar img {
  border-radius: 50%;
  border: 4px solid #fffafa;
  background: #000000;
}
@media (min-width: 960px) {
  .welcome-card {
    margin-left: 0;
    margin-right: auto;
  }
}
</style>
