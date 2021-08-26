<template>
  <!-- Button trigger modal -->
  <div class="position-absolute bottom-0 end-0">
    <MDBBtn color="light" size="lg" floating @click="staticBackdrop = true">
      <MDBIcon icon="cog" size="lg"></MDBIcon>
    </MDBBtn>
  </div>

  <!-- Modal -->
  <MDBModal
    id="staticBackdrop"
    tabindex="-1"
    labelledby="staticBackdropLabel"
    v-model="staticBackdrop"
    staticBackdrop
  >

    <MDBModalTitle id="staticBackdropLabel" class="text-start ps-3 pt-3 text-primary"> Settings </MDBModalTitle>
    <MDBModalBody>

      <MDBInput 
       class="my-4"
       type="number"
       v-for="timer in updatedTimers"
       :key="timer"
       :label="timer.name[0].toUpperCase() + timer.name.slice(1)"
       v-model="timer.seconds"
        />
      
    </MDBModalBody>
    <MDBModalFooter>
      <MDBBtn color="light" @click="staticBackdrop = false"> Close </MDBBtn>
      <MDBBtn color="primary" @click="save(updatedTimers);staticBackdrop = false"> Save </MDBBtn>
    </MDBModalFooter>
  </MDBModal>
</template>

<script>
  import {
    MDBModal,
    MDBModalTitle,
    MDBModalBody,
    MDBModalFooter,
    MDBBtn,
    MDBIcon,
    MDBInput,
  } from 'mdb-vue-ui-kit';
  import { ref } from 'vue';

  export default {
    components: {
      MDBModal,
      MDBModalTitle,
      MDBModalBody,
      MDBModalFooter,
      MDBBtn,
      MDBIcon,
      MDBInput,
    },
    setup() {
      const staticBackdrop = ref(false);

      return {
        staticBackdrop,
      };
    },
    props: {
      timers: Array,
      save: Function,
    },
    data() {
      return{
        updatedTimers: [],
      }
    },
    mounted() {
      this.updatedTimers = this.timers.map((t, i) => {
        return {...t, seconds: this.timers[i]['seconds'] / 60}
      })
    }
  };
</script>