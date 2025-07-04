<template>
  <v-container class="form-bg" fluid>
     <welcome/>
     <context1/>
     

  </v-container>
</template>

<script>

import welcome from '~/components/welcome.vue';
import context1 from '~/components/context1.vue';
export default {
  name: 'IndexPage',
  components: {welcome,context1}
};

</script>

