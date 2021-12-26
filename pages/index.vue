<template>
  <div>
    <Navigation />
    <Table />
  </div>
</template>

<script>
import Navigation from '../components/Navigation.vue'
import Table from '../components/Table.vue'

export default {
  name: 'IndexPage',
  components: { Navigation, Table },
}
</script>
