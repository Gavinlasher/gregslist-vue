<template>
  <div class="row bg-dark">
    <router-link :to="{ name: 'HouseDetails', params: { id: houses.id } }">
      <div class="col-12">
        <div class="">
          <img class="img-fluid" :src="houses.imgUrl" alt="" />
        </div>
        <div class="text-center">
          {{ houses.price }}
        </div>
        <div>
          <p>{{ houses.bathrooms }}| {{ houses.bedrooms }}</p>
        </div>
      </div>
    </router-link>
  </div>
</template>


<script>
export default {
  props: {
    houses: {
      type: Object,
      required: true,
    },
  },
  setup(props) {
    return {};
  },
};
</script>


<style lang="scss" scoped>
</style>