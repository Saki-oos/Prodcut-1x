<template>
    <v-container>
        <v-card v-if="product" class="mx-auto" max-width="500">
            <v-img :src="product.image" height="450px"></v-img>
            <v-card-title>{{ product.name }}</v-card-title>
          
            <v-card-text>
                <div>{{ product.description }}</div>
                <div>{{ product.description2 }}</div>
                 <div>{{ product.description3 }}</div>
                  <div>{{ product.description4 }}</div>
                   <div>{{ product.description5 }}</div>
            </v-card-text>
        </v-card>
       
          
        </v-alert>
    </v-container>
</template>

<script>
export default {
    async asyncData({ params }) {
    
        const products = [
            { id: 1, name: 'เสื้อเจอร์ซี่พิมพ์ลายทั้งตัว PUMA x SQUID GAME สำหรับผู้ชาย',
              price: 100, 
              description: 'รายละเอียด ' ,
              description2: ' ฿ 3,000.00 ' ,
              description3: ' เหลือไม่เยอะ ' ,
              description4: ' เมื่อซื้อรองเท้า รับส่วนลด20% สำหรับซื้อถุงเท้าในใบเสร็จเดียวกัน *ไม่ร่วมกับโปรโมชั่นอื่นฟรีพวงกุญแจ SQUID GAME* ' ,
              description5: '  รายการนี้จัดส่งฟรี * ' ,
               image: '/02.png' 
            },
            { id: 2, name: 'นาย ชุติมัน จักรขจรชัย ', price: 200, 
             description: 'รายละเอียด ' ,
              description2: ' ฿4,600  ' ,
              description3: ' ประเทศ/ภูมิภาคที่ผลิต: จอร์เจีย' ,
              description4: ' สีที่แสดง: Rush Blue/ขาว/Speed Red/ขาว' ,
              description5: ' สไตล์: HJ4543-496 ' ,
            image: '/03.png' },
            { id: 3, name: 'เสื้อนักแข่งทีม MERCEDES - AMG PETRONAS FORMULA ONE', price: 200, 
                description: 'รายละเอียด',
                description2: 'This product is excluded from all promotional discounts and offers. ' ,
                description3: ' Black / White ' ,
                description4: 'สินค้ามีขนาดตามจริงเราแนะนำให้สั่งซื้อสินค้าตามขนาดปกติของคุณ ' ,
                description5: 'ผู้ชาย • Motorsport ' ,
               image: '/04.png' },
        ];
        const product = products.find(p => p.id == params.id);
        return { product };
    }
}
</script>