<template>
  <div class="contact-fixed">
    <transition name="contact-fade">
      <v-card v-if="expanded" elevation="6" class="contact-mini-card">
        <v-card-title class="contact-mini-title d-flex justify-space-between align-center">
          <span>ติดต่อเรา</span>
          <v-btn icon small @click="expanded = false" class="close-btn"><v-icon>mdi-chevron-up</v-icon></v-btn>
        </v-card-title>
        <v-card-text class="contact-mini-text contact-mini-flex">
          <div class="contact-row">
            <v-icon left color="primary">mdi-email</v-icon>
            <span class="contact-email">krittanai2548@gmail.com</span>
          </div>
          <div class="contact-row mt-2">
            <v-icon left color="primary">mdi-phone</v-icon>
            <span class="contact-phone">0960317654</span>
          </div>
        </v-card-text>
      </v-card>
      <v-btn v-else icon class="contact-expand-btn" @click="expanded = true">
        <v-icon color="primary">mdi-chevron-down</v-icon>
      </v-btn>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'ContactMini',
  data() {
    return {
      expanded: true,
    };
  },
};
</script>

<style scoped>
.contact-fixed {
  position: fixed;
  top: 100px;
  right: 32px;
  z-index: 1000;
  display: flex;
  justify-content: flex-end;
  align-items: flex-start;
}
.contact-mini-card {
  min-width: 300px;
  max-width: 300px;
  border-radius: 14px;
  background: rgba(0, 0, 0, 0.95);
  box-shadow: 0 4px 18px 0 rgba(80, 80, 160, 0.13);
  transition: box-shadow 0.2s, transform 0.2s;
}
.contact-mini-card:hover {
  box-shadow: 0 8px 32px 0 #7c4dff33;
  transform: translateY(-2px) scale(1.03);
}
.contact-mini-title {
  font-size: 1.08rem;
  font-weight: bold;
  color: #1976d2;
  padding-bottom: 0;
  padding-top: 8px;
  letter-spacing: 0.5px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.close-btn {
  margin-left: 8px;
  color: #7c4dff;
}
.contact-mini-text {
  font-size: 0.97rem;
  color: #fff;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding-top: 0;
  padding-bottom: 12px;
}
.contact-row {
  display: flex;
  align-items: center;
  margin-bottom: 2px;
}
.contact-email {
  color: #7c4dff;
  font-weight: 500;
  margin-left: 4px;
  word-break: break-all;
}
.contact-phone {
  color: #7c4dff;
  font-weight: 500;
  margin-left: 4px;
  word-break: break-all;
}
.mt-2 {
  margin-top: 8px;
}
.contact-expand-btn {
  background: rgba(255, 255, 255, 0.95);
  border-radius: 50%;
  box-shadow: 0 2px 8px 0 #00ffc322;
  transition: box-shadow 0.2s;
}
.contact-expand-btn:hover {
  box-shadow: 0 4px 16px 0 #7c4dff33;
}
.contact-fade-enter-active, .contact-fade-leave-active {
  transition: opacity 0.25s, transform 0.25s;
}
.contact-fade-enter, .contact-fade-leave-to {
  opacity: 0;
  transform: scale(0.9);
}
</style>
