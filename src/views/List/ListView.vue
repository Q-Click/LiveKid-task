<template>
   <div class="p-2">
      <div class="field">
         <div class="control">
            <input
               @input="onInput"
               placeholder="Type here..."
               type="text"
               class="input"
            />
         </div>
      </div>
      <Table :content="tableContent" :config="tableConfig" @select="onRowClicked" />
      <Modal ref="editorModal" @save="onSave"/>
   </div>
</template>
<script>
import Table from '@/components/Table.vue'
import Modal from '@/components/Modal.vue'
import { computed, onMounted, reactive } from 'vue'
import { filterList, mapList } from './listHelper'
import dummy from '@/assets/dummy.json'
import timeout from 'q'
export default {
   components: { Table, Modal },
   setup() {
      const tableConfig = {
         columns: [
            { key: 'id', header: 'ID' },
            { key: 'name', header: 'Name' },
            { key: 'cords_display', header: 'Cords' },
            { key: 'tags_display', header: 'Tags' },
            { key: 'status', header: 'Status' }
         ]
      }
      const state = reactive({
         items: [],
         initLoading: true,
         search: '',
         timeout
      })
      const tableContent = computed(() =>
         state.items.filter(item => filterList(item, state.search)).map(mapList)
      )
      const onInput = ({ target: { value } }) => {
         clearTimeout(timeout)
         state.timeout = setTimeout(() => (state.search = value), 500)
      }
      const mockRequest = () => {
         return new Promise(resolve => {
            setTimeout(() => {
               state.items = dummy
               resolve()
            }, 500)
         })
      }
      onMounted(async () => {
         await mockRequest()
         state.loading = false
      })      
      
      return { tableContent, tableConfig, onInput, state }
   },
   methods: {
       onRowClicked(value) {
        this.$refs.editorModal.showModal(value);
      },
       onSave(value) {
         const item = this.state.items.find(item => item.id === value.id)
         item.name = value.newName
         item.status = value.newStatus
      }
   }
}
</script>
