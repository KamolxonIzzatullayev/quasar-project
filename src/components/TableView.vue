<template>
      <q-page>
    <div class="q-pa-md">
    <q-table
      title="Products"
      :data="data"
      :columns="columns"
      row-key="name"
      :loading="loading"
      :filter="filter"
      binary-state-sort
    >
    <template v-slot:top-right>
        <q-input borderless outlined dense debounce="300" v-model="filter" placeholder="Search">
          <template v-slot:append>
            <q-icon name="search" />
          </template>
        </q-input>
          <q-btn class="q-ml-md" @click="addItem" color="primary" icon="add" label="Add product" />
      </template>
    <template v-slot:header="props">
        <q-tr :props="props">
          <q-th
            v-for="col in props.cols"
            :key="col.name"
            :props="props"
          >
            {{ col.label }}
          </q-th>
          <q-th auto-width> actions </q-th>
        </q-tr>
      </template> 

      <template v-slot:body="props">
        <q-tr :props="props">
          <q-td
            v-for="col in props.cols"
            :key="col.name"
            :props="props"
          >
            {{ col.value }}
          </q-td>
          <q-td auto-width>
            <q-btn @click="editRow(props)" class="q-ma-sm" size="md" color="amber" round dense icon="edit" />
            <q-btn @click="del(props)" class="q-ma-sm" size="md" color="red" round dense icon="delete" />
          </q-td>
        </q-tr>
      </template>

    </q-table>

  </div>
      </q-page>
</template>

<script>
import axios from 'axios';
import { Dialog } from 'quasar'
export default {
    data() {
        return {
            
      columns: [
        {
          name: 'id',
          required: true,
          label: 'id',
          align: 'left',
          field: row => row.id,
          format: val => `${val}`,
          sortable: true
        },
        { name: 'product_type_id', align: 'center', label: 'product_type_id', field: row => row.product_type_id},
        { name: 'name_uz',align: 'center', label: 'name_uz', field: row => row.name_uz },
        { name: 'cost',align: 'center', label: 'cost', field: row => row.cost },
        { name: 'address',align: 'center', label: 'address', field: row => row.address },
        { name: 'created_date',align: 'center', label: 'created_date', field: row => row.created_date, sortable: true}
      ],
      data: [],
      loading: false,
      filter: '',
      selected: [],
      deleteMessage: ''
        }
    },
  mounted() {
    axios.get('http://94.158.54.194:9092/api/product')
    .then((response) => {
      this.data = response.data;
    })
  },
  emits: ['editing', 'add'],
  methods: {
      editRow(e) {
          let newData = e.row
        this.$emit("editing", newData);
      },
      del(e) {
        this.$q.dialog({
            title: 'Confirm',
            message: 'Would you like to delete?',
            cancel: true,
            persistent: true
        }).onOk(() => {
            axios.delete(`http://94.158.54.194:9092/api/product/${e.row.id}`)
            .then(() => this.deleteMessage = 'Delete successful');
            // location.reload();
        }).onCancel(() => {
            // console.log('>>>> Cancel')
        }).onDismiss(() => {
            // console.log('I am triggered on both OK and Cancel')
        })
    },
      },
      addItem() {
          this.$emit("add")
      }
}
</script>

<style>

</style>