<template>
    <v-container class="form-bg" fluid>
        <prodcut01/>
        <context1/>
        <pro03/>
        
    </v-container>
    </template>
    <script>

import context1 from '~/components/context1.vue';
import prodcut01 from '~/components/prodcut01.vue'
import pro03 from './detail_product/pro03.vue';



export default {
  name: 'IndexPage2',
  components: {
    prodcut01,context1,pro03
    
  }
};

</script>

</template>