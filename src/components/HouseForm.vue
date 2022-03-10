<template>
  <form>
    <div class="form-group">
      <label for="bedrooms" class="">bedrooms</label>
      <input
        v-model="editable.bedrooms"
        placeholder="bedrooms"
        type="text"
        class="form-control"
        name="bedrooms"
        id="bedrooms"
        required
      />
    </div>
    <div class="form-group">
      <label for="bathrooms" class="">bathrooms</label>
      <input
        v-model="editable.bathrooms"
        placeholder="bathrooms"
        type="text"
        class="form-control"
        name="bathrooms"
        id="bathrooms"
        required
      />
    </div>
    <div class="form-group">
      <label for="levels" class="">levels</label>
      <input
        v-model="editable.levels"
        placeholder="levels"
        type="text"
        class="form-control"
        name="levels"
        id="levels"
        required
      />
    </div>
    <div class="form-group">
      <label for="price" class="">Price:</label>
      <input
        v-model="editable.price"
        placeholder="Price"
        type="number"
        class="form-control"
        name="price"
        id="price"
        min="0"
        max="9999999"
      />
    </div>
    <div class="form-group">
      <label for="year" class="">year:</label>
      <input
        v-model="editable.year"
        placeholder="year"
        type="year"
        class="form-control"
        name="year"
        id="year"
      />
    </div>

    <div class="form-group">
      <label for="img" class="">img:</label>
      <input
        v-model="editable.imgUrl"
        placeholder="https://imgurl.com"
        type="url"
        class="form-control"
        name="img"
        id="img"
        required
      />
    </div>
    <div class="d-flex justify-content-between my-3">
      <button
        type="button"
        data-bs-dismiss="modal"
        aria-label="Close"
        class="btn text-dark lighten-20 text-uppercase selectable"
      >
        <b> cancel </b>
      </button>
      <button
        @click="createHouse"
        type="button"
        class="btn btn-success text-dark text-uppercase selectable"
      >
        <b> Create House </b>
      </button>
      <button
        @click="editHouse"
        type="button"
        class="btn btn-info text-warning text-uppercase selectable"
      >
        <b> Edit House </b>
      </button>
    </div>
  </form>
</template>


<script>
import { ref } from "@vue/reactivity";
import { logger } from "../utils/Logger";
import { housesService } from "../services/HousesService";
import { Modal } from "bootstrap";
import { useRouter } from "vue-router";
import { watchEffect } from "@vue/runtime-core";
export default {
  props: {
    houseData: {
      type: Object,
      required: false,
    },
  },
  setup(props) {
    const router = useRouter();
    const editable = ref({});
    watchEffect(() => {
      editable.value = props.houseData;
    });
    return {
      editable,
      async createHouse() {
        try {
          let newHouse = await housesService.createHouse(editable.value);
          editable.value = {};
          Modal.getOrCreateInstance(
            document.getElementById("form-modal")
          ).hide();
          router.push({ name: "HousesDetails", params: { id: newHouse.id } });
        } catch (error) {
          logger.error(error);
        }
      },
      async editHouse() {
        try {
          await housesService.editHouse(editable.value);
          Modal.getOrCreateInstance(
            document.getElementById("form-modal")
          ).hide();
        } catch (error) {
          logger.error(error);
        }
      },
    };
  },
};
</script>


<style lang="scss" scoped>
</style>