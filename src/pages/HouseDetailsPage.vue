<template>
  <div class="component container">
    <div class="row p-4 bg-dark">
      <div class="col-6">
        <img class="img-fluid" :src="house.imgUrl" alt="" />
      </div>
      <div class="col-6">
        <p>{{ house.price }}</p>
        <p>{{ house.bedrooms }}| {{ house.bathrooms }}</p>
      </div>
      <div class="text-end selectable" @click="openModal">
        Edit House
        <i class="mdi mdi-plus"></i>
      </div>
    </div>
  </div>
  <Modal>
    <template #modal-title
      >Edit {{ house.price }} {{ house.bedrooms }}</template
    >
    <template #modal-body><HouseForm :houseData="house" /></template>
  </Modal>
</template>


<script>
import { computed, onMounted } from "@vue/runtime-core";
import { useRoute } from "vue-router";
import { AppState } from "../AppState";
import { logger } from "../utils/Logger";
import { housesService } from "../services/HousesService";
import { Modal } from "bootstrap";
export default {
  setup() {
    const route = useRoute();
    onMounted(async () => {
      try {
        AppState.activeCar = {};
        logger.log(route.params.id);
        await housesService.getById(route.params.id);
      } catch (error) {
        logger.error(error);
      }
    });
    return {
      house: computed(() => AppState.activeHouse),
      openModal() {
        Modal.getOrCreateInstance(document.getElementById("form-modal")).show();
      },
    };
  },
};
</script>


<style lang="scss" scoped>
</style>