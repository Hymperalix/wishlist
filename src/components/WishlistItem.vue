<template>
  <q-item>
    <q-item-main>
      <q-input v-if="item_edit" v-model="item.text" ref="item_text" autofocus @focus="$refs.item_text.select()" @keyup.enter="item_edit = false"/>
      <div v-else>{{ item.text }}</div>
    </q-item-main>
    <q-item-side right>
      <q-btn :icon="item_edit ? 'check' : 'edit'" :color="item_edit ? 'green' : 'orange'" flat @click="item_edit = !item_edit">
        <q-tooltip>
          Edit
        </q-tooltip>
      </q-btn>
      <q-btn icon="delete" color="negative" flat @click="delete_item(i)">
        <q-tooltip>
          Delete
        </q-tooltip>
      </q-btn>
    </q-item-side>
  </q-item>
</template>

<script>
export default {
  props: ['item', 'i'],
  data: () => ({
    item_edit: false
  }),
  watch: {
    item_edit: function(newValue) {
      if (!newValue)
        this.$emit('update');
    }
  },
  methods: {
    delete_item (i) {
      this.$q.dialog({
        title: 'Wishlist',
        message: 'Are you sure you wish to delete "' + this.item.text + '"?',
        cancel: true,
        color: 'primary',
        ok: {
          label: 'Delete',
          color: 'negative'
        }
      }).then(() => {
        this.$emit('delete', i);
      }).catch(() => {

      })      
    }
  }
}
</script>