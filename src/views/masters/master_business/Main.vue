<template>
    <TabGroup>
      <TabList class="nav-boxed-tabs mt-10">
          <Tab class="w-full py-2 " tag="button">Manage Business</Tab>
          <Tab class="w-full py-2" tag="button">Manage Services</Tab>
          <Tab class="w-full py-2" tag="button">Manage Categories</Tab>
      </TabList>
      <TabPanels class="mt-5">
          <TabPanel class="leading-relaxed">
            <Business></Business>
          </TabPanel>
          <TabPanel class="leading-relaxed">
          <Services></Services>
          </TabPanel>
          <TabPanel class="leading-relaxed">
          <Categories></Categories>
          </TabPanel>
      </TabPanels>
  </TabGroup>
  </template>
  
  <script>
  import Business from "./business/Main.vue"
  import Services from "./services/Main.vue"
  import Categories from "./categories/Main.vue"

  export default {
    data() {
      return {};
    },
    components : {
      Business,
      Categories,
      Services
    },
  };
  </script>
  