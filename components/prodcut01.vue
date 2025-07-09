<template>
  <v-container class="product-bg" fluid>
    <div class="bg-shapes"></div>
    <v-row align="center" justify="center" style="min-height: 100vh; position: relative; z-index: 2;">
      <v-col cols="12" md="10" lg="8">
        <v-card elevation="10" class="pa-8 product-card">
          <v-card-title class="product-title mb-6">
            <v-icon left color="deep-purple accent-4" size="36">mdi-cart</v-icon>
            สินค้าของเรา
          </v-card-title>
          <v-row>
            <v-col v-for="(item, idx) in products" :key="idx" cols="12" sm="6" md="4">
              <v-card class="product-item" elevation="4">
                <v-img :src="item.image" height="180px" class="mb-2"></v-img>
                <v-card-title class="product-name">{{ item.name }}</v-card-title>
                <v-card-subtitle class="product-price">{{ item.price | currency }}</v-card-subtitle>
                <v-card-text class="product-desc">{{ item.desc }}</v-card-text>
                <v-card-actions>
                  <v-btn color="success" block>สั่งซื้อ</v-btn>
                </v-card-actions>
              </v-card>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'ProductPage',
  data() {
    return {
      products: [
        {
          name: 'เสื้อยืดลายแมว',
          price: 299,
          desc: 'เสื้อยืดผ้าคอตตอน 100% สวมใส่สบาย ลายแมวน่ารัก',
          image: 'https://images.unsplash.com/photo-1512436991641-6745cdb1723f?auto=format&fit=crop&w=400&q=80',
        },
        {
          name: 'แก้วน้ำลายกาแล็กซี่',
          price: 199,
          desc: 'แก้วน้ำเก็บความเย็นลายกาแล็กซี่ สีสันสดใส',
          image: 'https://images.unsplash.com/photo-1506744038136-46273834b3fb?auto=format&fit=crop&w=400&q=80',
        },
        {
          name: 'หมวกแฟชั่น',
          price: 159,
          desc: 'หมวกแฟชั่นสไตล์เกาหลี ใส่ได้ทุกโอกาส',
          image: 'https://images.unsplash.com/photo-1465101046530-73398c7f28ca?auto=format&fit=crop&w=400&q=80',
        },
      ],
    };
  },
  filters: {
    currency(val) {
      return val ? val.toLocaleString('th-TH', { style: 'currency', currency: 'THB' }) : '';
    },
  },
};
</script>

<style scoped>
.colorful-card {
  border-radius: 22px;
  background: linear-gradient(120deg,  #000000 100%);
  box-shadow: 0 8px 32px 0 rgba(124, 77, 255, 0.12), 0 1.5px 8px 0 #ff80ab33;
  position: relative;
  z-index: 2;
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
  opacity: 0.25;
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
@keyframes floatShape {
  0% { transform: translateY(0) scale(1); }
  100% { transform: translateY(40px) scale(1.08); }
}
.product-card {
  border-radius: 22px;
  background: #000000;
  box-shadow: 0 8px 32px 0 rgba(124, 77, 255, 0.10), 0 1.5px 8px 0 #80d8ff33;
  position: relative;
  z-index: 2;
}
.product-title {
  font-size: 2rem;
  font-weight: bold;
  color: #512da8;
  letter-spacing: 1px;
  display: flex;
  align-items: center;
}
.product-item {
  border-radius: 16px;
  transition: box-shadow 0.2s, transform 0.2s;
  box-shadow: 0 2px 12px 0 #7c4dff22;
  background: linear-gradient(120deg, #000000 0%, #000000 100%);
}
.product-item:hover {
  box-shadow: 0 8px 32px 0 #7c4dff33;
  transform: translateY(-2px) scale(1.03);
}
.product-name {
  font-size: 1.15rem;
  font-weight: 600;
  color: #1976d2;
}
.product-price {
  color: #00bfae;
  font-weight: 500;
  font-size: 1.08rem;
}
.product-desc {
  color: #333;
  font-size: 0.98rem;
  min-height: 48px;
}
</style>
