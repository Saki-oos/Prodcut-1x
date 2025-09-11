<template>
  <v-container>
    <v-row justify="center" align="center" style="min-height: 80vh;">
      <v-col cols="12" md="8" class="text-center">
        <h1 class="mb-6">📊 Admin Dashboard</h1>
        <p>ยินดีต้อนรับ, คุณคือผู้ดูแลระบบ</p>
        <v-btn color="deep-purple accent-4" dark @click="$router.push('/login')">
          <v-icon left>mdi-logout</v-icon> ออกจากระบบ
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "AdminDashboard"
};
</script>
