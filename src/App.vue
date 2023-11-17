<template>
  <div id="app" class="container mx-auto p-4">
    
      <Tabs />
    
    
  </div>
</template>

<script>
import Tabs from '@/components/TabsComponent.vue';


export default {
  components: {
    Tabs,
  },
};
</script>

<style>
/* Add your global styles here */
.width-global{
  width: 50%;
}
</style>