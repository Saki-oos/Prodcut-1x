<template>
  <v-container fluid class="product-bg py-10">
    <v-row>
      <v-col cols="12">
        <h1 class="text-center mb-8 display-1 font-weight-bold text-primary">
          สินค้าของเรา
        </h1>
      </v-col>
    </v-row>
    <v-row>
      <v-col
        v-for="product in products"
        :key="product.id"
        cols="12"
        sm="6"
        md="4"
        class="d-flex"
      >
        <v-card class="mx-auto product-card" elevation="12">
          <v-img
            :src="product.image"
            height="220px"
            class="rounded-t"
            gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.3)"
          ></v-img>
          <v-card-title class="headline font-weight-bold text-secondary">
            {{ product.name }}
          </v-card-title>
          <v-card-subtitle class="mb-2">
            <v-icon left color="primary" class="mr-1">mdi-store</v-icon>
            {{ product.brand }}
          </v-card-subtitle>
          <v-card-text>
            <div class="mb-2">
              <v-icon left color="accent" class="mr-1">mdi-cash</v-icon>
              <span class="font-weight-bold">ราคา:</span>
              <span> {{ product.price | currency }} </span>
            </div>
            <div v-if="product.description" class="grey--text">
              {{ product.description }}
            </div>
          </v-card-text>
          <v-divider></v-divider>
          <v-card-actions>
            <v-btn :to="'/pro3/' + product.id" color="success" rounded block>
              <v-icon left>mdi-cart</v-icon>
              สั่งซื้อ
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "ProductPage",
  data() {
    return {
      products: [
        {
          id: 1,
          name: "เสื้อเจอร์ซี่พิมพ์ลายทั้งตัว PUMA x SQUID GAME สำหรับผู้ชาย",
          brand: "PUMA",
          price: 3000,
          image: "02.png",
          description: "เสื้อยืดผ้าคอตตอน 100% สวมใส่สบาย "
          
        },
        {
          id: 2,  
          name: "Chelsea FC 2025/26 Match Home",
          brand: "Nike ",
          price: 4600,
          image: "03.png",
          description: "เสื้อแข่งฟุตบอล Authentic ผู้ชาย Nike Dri-FIT ADV"
        },
        {
          id: 3,
          name: "เสื้อนักแข่งทีม MERCEDES - AMG PETRONAS FORMULA ONE",
          brand: "adidas",
          price: 4560,
          image: "04.png",
          description: "This product is excluded from all promotional discounts and offers."
        }
      ]
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
.product-bg {
  min-height: 100vh;
  background: linear-gradient(135deg, #000000 0%, #000000 100%);
}
.product-card {
  border-radius: 20px;
  transition: box-shadow 0.3s;
}
.product-card:hover {
  box-shadow: 0 8px 32px 0 rgba(0, 123, 255, 0.25);
  transform: translateY(-4px) scale(1.03);
}
.rounded-t {
  border-top-left-radius: 20px !important;
  border-top-right-radius: 20px !important;
}
.text-center {
  text-align: center;
}
</style>