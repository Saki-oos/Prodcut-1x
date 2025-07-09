<template>
    <v-container class="form-bg" fluid>
        <Prodcut01/>
        <context1/>
        
    </v-container>
    </template>
    <script>

import context1 from '~/components/context1.vue';
import Prodcut01 from '~/components/prodcut01.vue';

export default {
  name: 'IndexPage2',
  components: {
    Prodcut01,context1
    
  }
};

</script>

</template>