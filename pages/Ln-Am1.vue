<template>
  <v-container class="form-bg" fluid>
     <ling_Am/>
     <context1/>
     

  </v-container>
</template>

<script>

import ling_Am from '~/components/Ling_Am.vue';
import context1 from '~/components/context1.vue';
export default {
  name: 'IndexPage',
  components: {ling_Am,context1}
};

</script>

