<template>
  <div v-if="asyncDataStatus_ready" class="col-full push-top">
    <CategoryList :categories="categories"/>
  </div>
</template>

<script>
//importimi i te dhenave dhe komponentit te kategorise ne PageHome
import {mapActions} from 'vuex' 
import CategoryList from '@/components/CategoryList'
import asyncDataStatus from '@/mixins/asyncDataStatus'
export default {
  components: {
    CategoryList
  },
  mixins: [asyncDataStatus],
  computed: {
//marja e kategorive nga state-i per ti renderuar ne komponent
    categories () {
      return Object.values(this.$store.state.categories) 
    }
  },
  methods: {
    ...mapActions(['fetchAllCategories', 'fetchForums'])
  },
  created () {
    //metoda per te bere fetch kategorite dhe forumet 
    this.fetchAllCategories()
      .then(categories => Promise.all(categories.map(category => this.fetchForums({ids: Object.keys(category.forums)}))))
      .then(() => {
        this.asyncDataStatus_fetched()
      })
  }
}

</script>