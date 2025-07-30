<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <h1 class="text-h4 mb-4">📚 รายชื่อหนังสือทั้งหมด</h1>
        <v-alert type="error" v-if="error">{{ error }}</v-alert>
        <v-progress-circular v-if="loading" indeterminate color="primary" class="ma-4" />
        
        <v-data-table
          v-else
          :headers="headers"
          :items="books"
          class="elevation-1"
          :items-per-page="5"
        >
          <template v-slot:item.in_stock="{ item }">
            <v-chip :color="item.in_stock ? 'green' : 'red'" dark>
              {{ item.in_stock ? 'พร้อมจำหน่าย' : 'หมด' }}
            </v-chip>
          </template>
        </v-data-table>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      books: [],
      error: null,
      loading: true,
      headers: [
        { text: 'รหัส', value: 'book_id' },
        { text: 'ชื่อหนังสือ', value: 'title' },
        { text: 'ผู้เขียน', value: 'author' },
        { text: 'เผยแพร่', value: 'published_date' },
        { text: 'ISBN', value: 'isbn' },
        { text: 'หน้า', value: 'pages' },
        { text: 'ราคา', value: 'price' },
        { text: 'สถานะ', value: 'in_stock' }
      ]
    }
  },
  async mounted() {
    try {
      const res = await fetch('http://localhost/library_pytdb/book_select.php')  // เปลี่ยน URL ตามจริง
    
      const json = await res.json()
      if (json.status === 'success') {
        this.books = json.data
         console.log('Response:', json.data);
      } else {
        this.error = json.message || 'เกิดข้อผิดพลาดในการโหลดข้อมูล'
      }
    } catch (err) {
      this.error = 'ไม่สามารถเชื่อมต่อกับ API ได้'
    } finally {
      this.loading = false
    }
  }
}
</script>

<style scoped>
h1 {
  font-weight: bold;
}
</style>
