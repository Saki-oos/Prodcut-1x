<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <h1 class="text-h4 mb-4">📚 รายชื่อหนังสือทั้งหมด</h1>
        <v-alert type="error" v-if="error">{{ error }}</v-alert>
        <v-progress-circular v-if="loading" indeterminate color="primary" class="ma-4" />
        <v-btn color="primary" class="mb-4" @click="addBook">
          <v-icon left>mdi-plus</v-icon> เพิ่มหนังสือ
        </v-btn>
        <template v-if="!loading">
          <v-data-table
            :headers="headersWithActions"
            :items="books"
            class="elevation-1"
            :items-per-page="5"
          >
            <template v-slot:item.in_stock="{ item }">
              <v-chip :color="item.in_stock ? 'green' : 'red'" dark>
                {{ item.in_stock ? 'พร้อมจำหน่าย' : 'หมด' }}
              </v-chip>
            </template>
            <template v-slot:item.actions="{ item }">
              <v-btn icon color="blue" @click="editBook(item)"><v-icon>mdi-pencil</v-icon></v-btn>
              <v-btn icon color="red" @click="deleteBook(item)"><v-icon>mdi-delete</v-icon></v-btn>
            </template>
          </v-data-table>
        </template>

        <v-dialog v-model="showDialog" max-width="500px">
          <v-card>
            <v-card-title>
              <span class="headline">{{ editMode ? 'แก้ไขหนังสือ' : 'เพิ่มหนังสือ' }}</span>
            </v-card-title>
            <v-card-text>
              <v-form ref="form" v-model="valid" lazy-validation>
                <v-text-field v-model="form.title" label="ชื่อหนังสือ" :rules="[v => !!v || 'กรุณาใส่ชื่อหนังสือ']" required></v-text-field>
                <v-text-field v-model="form.author" label="ผู้เขียน" :rules="[v => !!v || 'กรุณาใส่ผู้เขียน']" required></v-text-field>
                <v-text-field v-model="form.published_date" label="เผยแพร่ (YYYY-MM-DD)" :rules="[v => !!v || 'กรุณาใส่วันที่เผยแพร่']" required></v-text-field>
                <v-text-field v-model="form.isbn" label="ISBN" required></v-text-field>
                <v-text-field v-model.number="form.pages" label="หน้า" type="number" :rules="[v => !!v || 'กรุณาใส่จำนวนหน้า']" required></v-text-field>
                <v-text-field v-model.number="form.price" label="ราคา" type="number" :rules="[v => !!v || 'กรุณาใส่ราคา']" required></v-text-field>
                <v-switch v-model="form.in_stock" label="พร้อมจำหน่าย"></v-switch>
              </v-form>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn text @click="closeDialog">ยกเลิก</v-btn>
              <v-btn color="primary" @click="saveBook" :disabled="!valid">{{ editMode ? 'บันทึก' : 'เพิ่ม' }}</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
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
      showDialog: false,
      editMode: false,
      valid: true,
      form: {
        book_id: null,
        title: '',
        author: '',
        published_date: '',
        isbn: '',
        pages: 0,
        price: 0,
        in_stock: true
      },
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
  computed: {
    headersWithActions() {
      return [
        ...this.headers,
        { text: 'การจัดการ', value: 'actions', sortable: false }
      ];
    }
  },
  async mounted() {
    await this.fetchBooks();
  },
  methods: {
    async fetchBooks() {
      this.loading = true;
      this.error = null;
      try {
        const res = await this.$axios.$get('http://localhost/library_pytdb/book_select.php');
        if (res.status === 'success') {
          this.books = res.data;
        } else {
          this.error = res.message || 'เกิดข้อผิดพลาดในการโหลดข้อมูล';
        }
      } catch (err) {
        this.error = 'ไม่สามารถเชื่อมต่อกับ API ได้';
      } finally {
        this.loading = false;
      }
    },
    addBook() {
      this.editMode = false;
      this.resetForm();
      this.showDialog = true;
    },
    closeDialog() {
      this.showDialog = false;
      this.resetForm();
      if (this.$refs.form) {
        this.$refs.form.resetValidation();
      }
    },
    editBook(item) {
      this.form = {
        ...item,
        in_stock: item.in_stock === 1 // Convert tinyint to boolean
      };
      this.editMode = true;
      this.showDialog = true;
    },
    async saveBook() {
      if (!this.$refs.form.validate()) {
        return;
      }

      this.form.in_stock = this.form.in_stock ? 1 : 0; // Convert boolean to tinyint

      try {
        if (this.editMode) {
          // Update an existing book
          const res = await this.$axios.$put('http://localhost/library_pytdb/book_updata.php', this.form);
          console.log(this.form);
          console.log(res);


          if (res.status === 'success') {
            const idx = this.books.findIndex(b => b.book_id === this.form.book_id);
            if (idx !== -1) {
              this.$set(this.books, idx, { ...this.form });
            }
          } else {
            this.error = res.message || 'เกิดข้อผิดพลาดในการแก้ไขข้อมูล';
          }
        } else {
          // Add a new book
          const res = await this.$axios.$post('http://localhost/library_pytdb/book_insert.php', this.form);
          if (res.status === 'success') {
            this.books.unshift({ ...this.form, book_id: res.book_id });
          } else {
            this.error = res.message || 'เกิดข้อผิดพลาดในการเพิ่มข้อมูล';
          }
        }
      } catch (err) {
        this.error = 'ไม่สามารถเชื่อมต่อกับ API ได้';
      }

      this.closeDialog();
    },
    async deleteBook(item) {
      if (confirm(`คุณต้องการลบหนังสือชื่อ "${item.title}" ใช่หรือไม่?`)) {
        try {
          const res = await this.$axios.$delete('http://localhost/library_pytdb/book_delete.php', {
            data: { book_id: item.book_id }
          });
          if (res.status === 'success') {
            this.books = this.books.filter(b => b.book_id !== item.book_id);
          } else {
            this.error = res.message || 'เกิดข้อผิดพลาดในการลบข้อมูล';
          }
        } catch (err) {
          this.error = 'ไม่สามารถเชื่อมต่อกับ API ได้';
        }
      }
    },
    resetForm() {
      this.form = {
        book_id: null,
        title: '',
        author: '',
        published_date: '',
        isbn: '',
        pages: 0,
        price: 0,
        in_stock: true
      };
    }
  }
}
</script>

<style scoped>
h1 {
  font-weight: bold;
}
</style>