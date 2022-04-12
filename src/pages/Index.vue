<template>
  <q-page>
  <div class="q-gutter-y-md">

      <q-tab-panels v-model="panel" animated class="shadow-2 rounded-borders">
        <q-tab-panel name="all">
          <table-view @editing="editedRow" @add="addProduct" /> 
        </q-tab-panel>

        <q-tab-panel name="edit">
          <edit-table :data='props' @back="panel = 'all'" />
        </q-tab-panel>

        <q-tab-panel name="add">
          <add-table @back="panel = 'all'" />
        </q-tab-panel>
      </q-tab-panels>
    </div>
  </q-page>
</template>

<script>

import TableView from 'src/components/TableView.vue';
import EditTable from 'src/components/EditTable.vue';
import AddTable from 'src/components/AddTable.vue';

export default {
  components: { TableView, EditTable, AddTable },
  name: 'PageIndex',
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
      panel: 'all',
      props: undefined
    }
  },
  methods: {
    editedRow(e) {
      this.panel = 'edit';
      this.props = e;
    },
    addProduct() {
      this.panel = 'add'
    }
  },
}
</script>
