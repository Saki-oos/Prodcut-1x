<template>
  <v-container class="form-bg" fluid>
     
     <context2/>
     <context1/>
     

  </v-container>
</template>

<script>

import context1 from '~/components/context1.vue';
import context2 from '~/components/context2.vue';

export default {
  name: 'IndexPage',
  components: {context2,context1}
};

</script>

