<template>
  <v-container class="welcome-bg animated-bg" fluid>
    <div class="bg-shapes"></div>
    <v-row align="center" justify="center" style="min-height: 100vh; position: relative; z-index: 2;">
      <v-col cols="12" md="8" lg="6">
        <v-card elevation="12" class="pa-10 welcome-card">
          <v-row align="center" justify="center">
            <v-avatar size="100" class="mb-6">
              <img src="/v.png" alt="Logo" />
            </v-avatar>
          </v-row>
          <v-card-title class="justify-center display-1 font-weight-bold welcome-title mb-4">
            ยินดีต้อนรับ
          </v-card-title>
          <v-card-text class="text-center mb-6">
            <div class="subtitle-1 mb-2">ระบบนี้ช่วยให้คุณสมัครสมาชิกได้อย่างรวดเร็วและปลอดภัย</div>
            <div class="mb-4">กรุณาคลิกปุ่มด้านล่างเพื่อเริ่มต้นการสมัครสมาชิก</div>
          </v-card-text>
          <v-row justify="center">
            <v-btn color="deep-purple accent-4" large class="welcome-btn" to="/loing">
              สมัครสมาชิก 
              <v-icon left> mdi-account-plus</v-icon>
           
               
            
            
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
.welcome-bg {
  min-height: 100vh;
  overflow: hidden;
  position: relative;
}
.animated-bg {
  background: linear-gradient(270deg, #dc00fe, #003cff, #ff00ea, #ff0073, #1100ff);
  background-size: 200% 200%;
  animation: gradientMove 8s ease-in-out infinite;
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
  opacity: 0.35;
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
