<template>
  <form>
    <div class="form-group">
      <label for="make" class="">Make:</label>
      <select
        v-model="editable.make"
        name="make"
        id="make"
        required
        class="form-control"
      >
        <option disabled :selected="!editable.id" value="">
          Please Choose your car make
        </option>
        <option>honda</option>
        <option>hyundai</option>
        <option>tesla</option>
        <option>toyota</option>
        <option>other</option>
      </select>
    </div>
    <div class="form-group">
      <label for="model" class="">Model:</label>
      <input
        v-model="editable.model"
        placeholder="Model"
        type="text"
        class="form-control"
        name="model"
        id="model"
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
      <label for="description" class="">description:</label>
      <textarea
        v-model="editable.description"
        placeholder="Description"
        type="text"
        class="form-control"
        name="description"
        id="description"
        rows="5"
      ></textarea>
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
        v-if="!carData.id"
        @click="createCar"
        type="button"
        class="btn btn-success text-dark text-uppercase selectable"
      >
        <b> Create Car </b>
      </button>
      <button
        v-else
        @click="editCar"
        type="button"
        class="btn btn-info text-warning text-uppercase selectable"
      >
        <b> Edit Car </b>
      </button>
    </div>
  </form>
</template>


<script>
import { AppState } from "../AppState";
import { computed, reactive, onMounted, ref, watchEffect } from "vue";
import { logger } from "../utils/Logger";
import { carsService } from "../services/CarsService";
import Pop from "../utils/Pop";
import { Modal } from "bootstrap";
import { useRouter } from "vue-router";
export default {
  props: {
    carData: {
      type: Object,
      required: false,
      default: {},
    },
  },
  setup(props) {
    const router = useRouter();
    const editable = ref({});
    watchEffect(() => {
      logger.log("change happened re-running watch effect");
      editable.value = props.carData;
    });
    return {
      editable,
      async createCar() {
        try {
          logger.log("editable before service", editable.value);
          // NOTE we save the car response from the server here so we can push later
          let newCar = await carsService.create(editable.value);
          editable.value = {};
          Modal.getOrCreateInstance(
            document.getElementById("form-modal")
          ).hide();
          router.push({ name: "CarDetails", params: { id: newCar.id } });
        } catch (error) {
          logger.error(error);
          Pop.toast(error.message, "error");
        }
      },
      async editCar() {
        try {
          await carsService.update(editable.value);
          Modal.getOrCreateInstance(
            document.getElementById("form-modal")
          ).hide();
        } catch (error) {
          logger.error(error);
          Pop.toast(error.message, "error");
        }
      },
    };
  },
};
</script>


<style scoped>
</style>
