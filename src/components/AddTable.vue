<template>
  <q-page>
      <q-card class="my-card">
      <q-card-section>
          <q-btn @click="goBack" color="primary" icon="keyboard_backspace" label="Back" />
        <div class="q-pa-md">
          <q-input class="q-pa-md" outlined clearable v-model="newData.product_type_id" label="product_type_id" />
          <q-input class="q-pa-md" outlined clearable v-model="newData.name_uz" label="name_uz" />
          <q-input class="q-pa-md" outlined clearable v-model="newData.cost" label="cost" />
          <q-input class="q-pa-md" outlined clearable v-model="newData.address" label="address" />
          <q-input class="q-pa-md" outlined clearable v-model="newData.created_date" label="created_date" />
      </div>
      <q-btn icon="save" label="Save" color="primary" @click="edit" />
      </q-card-section>
    </q-card>
      
    </q-page>
</template>

<script>
import axios from 'axios';
import { Dialog } from 'quasar'
export default {
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
            this.$q.dialog({
                title: 'Confirm',
                message: 'Would you like to add a new product?',
                cancel: true,
                persistent: true
            }).onOk(() => {
                const article = { 
                    id: this.newData.id,
                    product_type_id: this.newData.product_type_id,
                    name_uz: this.newData.name_uz,
                    cost: this.newData.cost,
                    address: this.newData.address,
                    created_date: this.newData.created_date,
                };
                axios.post(`http://94.158.54.194:9092/api/product`, article)
                .then(response => this.res = response)
                this.newData = {};
            }).onOk(() => {
                
            }).onCancel(() => {
            // console.log('>>>> Cancel')
            }).onDismiss(() => {
            // console.log('I am triggered on both OK and Cancel')
            })
            
        },
    }
}
</script>

<style>

</style>