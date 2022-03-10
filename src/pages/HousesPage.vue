<template>
    <div class="house-page container-fluid">
 <b
      title="Create House"
      class="
        create-btn
        btn btn-dark
        rounded-pill
        shadow
        d-flex
        align-items-center
        justify-content-center
      "
      data-bs-toggle="modal"
      data-bs-target="#form-modal"
    >
      <i class="mdi mdi-plus"></i>
    </b>
<div class="row">
    <div v-for="h in houses" :key="h.id" class="col-4 p-4">
<House  :houses="h"/>
    </div>
</div>
  <Modal>
      <template #modal-title>Create House</template>
      <template #modal-body><HouseForm /></template>
    </Modal>

    </div>
</template>


<script>
import { computed, onMounted } from '@vue/runtime-core'
import {housesService} from '../services/HousesService'
import { logger } from '../utils/Logger'
import { AppState } from '../AppState'
export default {
    setup(){
        onMounted(async () => {
            try {
                housesService.getAll()
            } catch (error) {
                logger.error(error)
            }
        }
        )
        return {
            houses: computed(() => AppState.houses)
        }
    }
}
</script>


<style lang="scss" scoped>
.create-btn{
    font-size: 20px;
  height: 10vh;
  width: 10vh;
  z-index: 100;
  position: fixed;
  bottom: 10vh;
  right: 5vh
    
}
</style>