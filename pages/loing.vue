<template>
    <v-container class="form-bg" fluid>
        <COM_jum/>
        <context1/>
        <pro03/>
        
    </v-container>
    </template>
    <script>
import COM_jum from '~/components/COM_jum.vue';
import context1 from '~/components/context1.vue';
import pro03 from './detail_product/pro03.vue';

export default {
  name: 'IndexPage2',
  components: {
    COM_jum,context1,pro03
    
  }
};

</script>

</template> --- IGNORE ---          