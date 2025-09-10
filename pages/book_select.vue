<template>
  <v-container class="mini-bookstore-bg py-8">
    <v-row>
      <v-col cols="12">
        <h1 class="text-h4 mb-4 mini-title">📚 ร้านขายหนังสือ MiniBook</h1>

        <v-alert type="error" v-if="error">{{ error }}</v-alert>
        <v-progress-circular v-if="loading" indeterminate color="primary" class="ma-4" />

        <v-text-field
          v-model="search"
          label="ค้นหาหนังสือ..."
          prepend-icon="mdi-magnify"
          class="mb-4 mini-search"
        ></v-text-field>

        <v-btn color="deep-purple accent-2" class="mb-4 mini-btn" @click="addBook">
          <v-icon left>mdi-plus</v-icon> เพิ่มหนังสือ
        </v-btn>

        <v-data-table
          v-if="!loading"
          :headers="headersWithActions"
          :items="filteredBooks"
          class="elevation-2 mini-table"
          :items-per-page="5"
        >
          <template v-slot:item.cover="{ item }">
            <v-img :src="item.cover || 'https://cdn.vuetifyjs.com/images/cards/sunshine.jpg'"
                   max-width="120" max-height="180" class="rounded-lg elevation-3 mx-auto book-cover-img" contain>
            </v-img>
          </template>

          <template v-slot:item.in_stock="{ item }">
            <v-chip :color="item.in_stock ? 'light-green accent-3' : 'red lighten-2'" dark class="mini-chip">
              {{ item.in_stock ? 'พร้อมเช่า' : 'หมด' }}
            </v-chip>
          </template>

          <template v-slot:item.actions="{ item }">
            <v-btn icon color="blue" @click="editBook(item)"><v-icon>mdi-pencil</v-icon></v-btn>
            <v-btn icon color="red" @click="deleteBook(item)"><v-icon>mdi-delete</v-icon></v-btn>
            <v-btn color="success" small class="ml-2" :disabled="!item.in_stock || item.quantity < 1" 
                   @click="openRentDialog(item)">
              <v-icon left>mdi-book-open-variant</v-icon> เช่าหนังสือ
            </v-btn>
          </template>
        </v-data-table>

        <!-- Dialog ฟอร์มเพิ่ม/แก้ไข -->
        <v-dialog v-model="showDialog" max-width="500px">
          <v-card>
            <v-card-title>
              <span class="headline">{{ editMode ? 'แก้ไขหนังสือ' : 'เพิ่มหนังสือ' }}</span>
            </v-card-title>
            <v-card-text>
              <v-form ref="form" v-model="valid" lazy-validation>
                <v-text-field v-model="form.title" label="ชื่อหนังสือ" required></v-text-field>
                <v-text-field v-model="form.author" label="ผู้เขียน" required></v-text-field>
                <v-text-field v-model="form.published_date" label="เผยแพร่ (YYYY-MM-DD)" required></v-text-field>
                <v-text-field v-model="form.isbn" label="ISBN" required></v-text-field>
                <v-text-field v-model.number="form.pages" label="หน้า" type="number" required></v-text-field>
                <v-text-field v-model.number="form.price" label="ราคาเช่าต่อวัน" type="number" required></v-text-field>
                <v-text-field v-model.number="form.quantity" label="จำนวนหนังสือ" type="number" required></v-text-field>
                <v-switch v-model="form.in_stock" label="พร้อมให้เช่า"></v-switch>
                <v-file-input
                  v-model="coverFile"
                  label="อัปโหลดรูปหนังสือ"
                  accept="image/*"
                  prepend-icon="mdi-image"
                  @change="uploadCover"
                />
                <v-text-field v-model="form.cover" label="URL รูปหนังสือ" prepend-icon="mdi-link"></v-text-field>
                <v-img v-if="form.cover" :src="form.cover" max-width="200" max-height="280" class="rounded-lg elevation-2 my-2 mx-auto book-cover-img"></v-img>
              </v-form>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn text @click="closeDialog">ยกเลิก</v-btn>
              <v-btn color="primary" @click="saveBook">บันทึก</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>

        <!-- Dialog เช่า -->
        <v-dialog v-model="rentDialog" max-width="400px">
          <v-card>
            <v-card-title>
              <span class="headline">เช่าหนังสือ: {{ rentForm.title }}</span>
            </v-card-title>
            <v-card-text>
              <v-form ref="rentFormRef" v-model="rentValid">
                <v-text-field v-model="rentForm.renter" label="ชื่อผู้เช่า" required></v-text-field>
                <v-text-field v-model="rentForm.return_date" label="วันที่คืน (YYYY-MM-DD)" required></v-text-field>
              </v-form>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn text @click="rentDialog=false">ยกเลิก</v-btn>
              <v-btn color="primary" @click="submitRent">ยืนยันเช่า</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>

        <!-- Snackbar แจ้งเตือนเช่าสำเร็จ -->
        <v-snackbar
          v-model="rentSuccess"
          :timeout="5000"
          color="success"
          top
          elevation="24"
          style="text-align:center; font-size:1.1rem;"
        >
          <div>
            <div>เช่าหนังสือสำเร็จ!</div>
            <div>ชื่อผู้เช่า: {{ rentForm.renter }}</div>
            <div>วันที่คืน: {{ rentForm.return_date }}</div>
            <div>เวลาเช่า: {{ rentTime }}</div>
          </div>
        </v-snackbar>

      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      books: [],
      error: null,
      loading: true,
      showDialog: false,
      editMode: false,
      valid: true,
      search: '',
      form: {
        book_id: null, title:'', author:'', published_date:'', isbn:'', pages:0,
        price:0, quantity:1, in_stock:true, cover:''
      },
      coverFile: null,
      rentDialog:false,
      rentForm:{ book_id:null, title:'', renter:'', return_date:'' },
      rentValid:true,
      rentSuccess: false,
      rentTime: '',
      headers: [
        { text:'รหัส', value:'book_id' },
        { text:'รูป', value:'cover', sortable:false },
        { text:'ชื่อหนังสือ', value:'title' },
        { text:'ผู้เขียน', value:'author' },
        { text:'เผยแพร่', value:'published_date' },
        { text:'ISBN', value:'isbn' },
        { text:'หน้า', value:'pages' },
        { text:'จำนวน', value:'quantity' },
        { text:'ราคาเช่าต่อวัน', value:'price' },
        { text:'สถานะ', value:'in_stock' }
      ]
    }
  },
  computed:{
    headersWithActions(){ return [...this.headers,{text:'การจัดการ', value:'actions', sortable:false}]; },
    filteredBooks(){
      if(!this.search) return this.books;
      const s = this.search.toLowerCase();
      return this.books.filter(b=> (b.title && b.title.toLowerCase().includes(s)) || 
                                   (b.author && b.author.toLowerCase().includes(s)) || 
                                   (b.isbn && b.isbn.toLowerCase().includes(s)));
    }
  },
  async mounted(){ await this.fetchBooks(); },
  methods:{
    async fetchBooks(){
      this.loading=true; this.error=null;
      try{
        const res = await axios.get('http://localhost/library_pytdb/book_select.php');
        if(res.status === 200 && Array.isArray(res.data)){
          this.books = res.data.map(b => ({
            ...b,
            book_id: b.id,
            quantity: b.stock,
            // ถ้าหนังสือหมด ให้ปิดสถานะพร้อมเช่า
            in_stock: (b.in_stock == 1) && (b.stock > 0)
          }));
        } else {
          this.error = 'เกิดข้อผิดพลาดในการโหลดข้อมูล';
        }
      }catch(err){ this.error='ไม่สามารถเชื่อมต่อกับ API ได้'; }
      finally{ this.loading=false; }
    },
    addBook(){ this.editMode=false; this.resetForm(); this.showDialog=true; this.coverFile=null; },
    closeDialog(){ this.showDialog=false; this.resetForm(); this.coverFile=null; },
    editBook(item){ 
      this.form={...item, in_stock: item.in_stock == 1}; // แปลงเป็น Boolean
      this.editMode=true; this.showDialog=true; this.coverFile=null; 
    },
    resetForm(){ 
      this.form={ book_id:null,title:'',author:'',published_date:'',isbn:'',pages:0,price:0,quantity:1,in_stock:true,cover:'' }; 
    },
    async uploadCover(file){
      if(!file) return;
      const formData = new FormData();
      formData.append('image', file);
      try {
        const res = await axios.post('http://localhost/library_pytdb/upload_image.php', formData, {
          headers: { 'Content-Type': 'multipart/form-data' }
        });
        if(res.data?.url){
          this.form.cover = res.data.url;
        }else{
          this.error = 'อัปโหลดรูปไม่สำเร็จ';
        }
      } catch (err) {
        this.error = 'อัปโหลดรูปไม่สำเร็จ';
      }
    },
    async saveBook(){
      try{
        const payload = {
          title: this.form.title,
          author: this.form.author,
          price: this.form.price,
          stock: this.form.quantity,
          published_date: this.form.published_date,
          isbn: this.form.isbn,
          pages: this.form.pages,
          cover: this.form.cover,
          in_stock: this.form.in_stock ? 1 : 0 // ส่งเป็น 1/0
        };
        if(this.editMode){
          payload.book_id = this.form.book_id;
          await axios.post('http://localhost/library_pytdb/book_updata.php', payload);
        }else{
          await axios.post('http://localhost/library_pytdb/book_insert.php', payload);
        }
        this.fetchBooks();
        this.closeDialog();
      }catch(err){ this.error='ไม่สามารถเชื่อมต่อกับ API ได้'; }
    },
    async deleteBook(item){
      if(confirm(`คุณต้องการลบ "${item.title}" ใช่หรือไม่?`)){
        try{
          await axios.post('http://localhost/library_pytdb/book_delete.php', {book_id:item.book_id});
          this.fetchBooks();
        }catch(err){ this.error='ไม่สามารถเชื่อมต่อกับ API ได้'; }
      }
    },
    openRentDialog(item){
      this.rentForm = {
        book_id: item.book_id,
        title: item.title,
        renter: '',
        return_date: ''
      };
      this.rentDialog = true;
    },
    async submitRent(){
      try{
        const res = await axios.post('http://localhost/library_pytdb/rentals_book.php', this.rentForm);
        if(res.data?.status==='success'){
          this.rentTime = new Date().toLocaleString('th-TH', { hour12: false });
          this.rentSuccess = true;
          this.fetchBooks();
          this.rentDialog=false;
        }else{
          alert(res.data?.message||'เกิดข้อผิดพลาดในการเช่า');
        }
      }catch(err){
        alert('ไม่สามารถเชื่อมต่อกับ API ได้');
      }
    }
  }
}
</script>

<style scoped>
.mini-title{ color:#000;font-weight:bold;text-align:center;margin-bottom:2rem; }
.mini-search{ 
  background: #ffb6e6; /* สีชมพู */
  border-radius: 12px;
  color: #111;
}
.mini-btn{ border-radius:20px;font-weight:bold;color:#000;background:linear-gradient(90deg,#a78bfa 60%,#f472b6 100%);}
.mini-table{ background:#ff87c9;border-radius:18px;color:#111; }
.book-cover-img{ border-radius:10px; box-shadow:0 2px 8px #8882; transition: transform 0.2s; }
.book-cover-img:hover{ transform: scale(1.08); }
</style>