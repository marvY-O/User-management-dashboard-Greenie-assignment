<template>
  <header class="mb-8">
    <div class="text-sm font-medium text-center text-gray-500 border-b border-gray-200 dark:text-gray-400 dark:border-gray-700">
      <ul class="flex flex-wrap -mb-px">
          <li v-for="(tab,index) in tabs"
          :key="index"
          @click="selectTab(index)"
          class="me-2">
            <a href="#" :class="{'inline-block p-4 border-b-2 border-transparent rounded-t-lg hover:text-gray-600 hover:border-gray-300 dark:hover:text-gray-300': activeTab!=index, 'inline-block p-4 text-orange-600 border-b-2 border-orange-600 rounded-t-lg active dark:text-orange-500 dark:border-orange-500': activeTab==index}"
            :aria-current="{'page':activeTab==index}">{{ tab }}</a>
          </li>
      </ul>
    </div>
  </header>
  <main>
    <div v-show="activeTab === 0" ><TableComponent/></div>
    <div v-show="activeTab === 1" ><FormComponent/></div>
    <!-- Your main content goes here -->
  </main>
  </template>
  
  <script>
  import TableComponent from '@/components/TableComponent.vue'
  import FormComponent from '@/components/FormComponent.vue'



  export default {
    components: {
      TableComponent, FormComponent
    },
    data() {
      return {
        tabs: ['User Details', 'Account Creation'],
        activeTab: 0,
      };
    },
    methods: {
      selectTab(index) {
        this.activeTab = index;
      },
    },
  };
  </script>
  