<template>
  <q-page>
      <q-card class="my-card">
      <q-card-section>
          <q-btn @click="goBack" color="primary" icon="keyboard_backspace" label="Back" />
        <div class="q-pa-md">
          <q-input class="q-pa-md" outlined clearable v-model="newData.id" label="id" />
          <q-input class="q-pa-md" outlined clearable v-model="newData.product_type_id" label="product_type_id" />
          <q-input class="q-pa-md" outlined clearable v-model="newData.name_uz" label="name_uz" />
          <q-input class="q-pa-md" outlined clearable v-model="newData.cost" label="cost" />
          <q-input class="q-pa-md" outlined clearable v-model="newData.address" label="address" />
          <q-input class="q-pa-md" outlined clearable v-model="newData.created_date" label="created_date" />
      </div>
      <q-btn @click="edit" align="right" color="primary" icon="save" label="Save" />
      </q-card-section>
    </q-card>
      
    </q-page>
</template>

<script>
import axios from 'axios';
import { Dialog } from 'quasar'
export default {
    props: ['data'],
    emits: ['back'],
    data() {
        return {
            newData: {},
            res: undefined
        }
    },
    methods: {
        goBack() {
            this.$emit('back')
        },
        edit() {
            const article = { 
                id: this.newData.id,
                product_type_id: this.newData.product_type_id,
                name_uz: this.newData.name_uz,
                cost: this.newData.cost,
                address: this.newData.address,
                created_date: this.newData.created_date,
            };
            axios.put(`http://94.158.54.194:9092/api/product?id=${this.newData.id}`, article)
            .then(response => this.res = response)
            
        }
    },
    computed() {
        },
    mounted() {
        this.newData = this.data;
    },
}
</script>

<style>

</style>