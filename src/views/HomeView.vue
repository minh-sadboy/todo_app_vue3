<template>
  <main class="mx-[10px] flex justify-center">
    <div>
      <div
        class="my-[20px] inline-block cursor-pointer rounded bg-blue-500 px-4 py-2 font-bold text-white hover:bg-blue-700"
        @click="showCreateModal = true"
      >
        Create
      </div>
      <div
        v-for="item in dataNote"
        class="mb-[15px]"
        @click="showEditModalFn(item)"
      >
        <NoteItem
          :title="item.title"
          :textContent="item.note"
          :id="item.id"
          @create="getDataNote"
        />
      </div>
    </div>
    <Transition>
      <Modal v-if="showEditModal">
        <NoteItem
          :title="itemEditModal.title"
          :textContent="itemEditModal.note"
          :id="itemEditModal.id"
          :modeEdit="true"
          @close="showEditModal = !showEditModal"
          @create="getDataNote"
        />
      </Modal>
    </Transition>
    <Transition>
      <Modal v-if="showCreateModal">
        <NoteItem
          title="Title"
          textContent="Note"
          :modeEdit="true"
          :modeCreate="true"
          @create="getDataNote"
          @close="showCreateModal = !showCreateModal"
        />
      </Modal>
    </Transition>
  </main>
</template>

<script setup>
import NoteItem from "../components/NoteItem.vue";
import Modal from "../components/Modal.vue";
import { ref, watch, onMounted, onUnmounted, inject } from "vue";

const showEditModal = ref(false);
const showCreateModal = ref(false);
const dataNote = ref([]);
const itemEditModal = ref({});
const searchText = inject("searchText");

watch(searchText, (newSearchText) => {
  if (newSearchText) {
    let data = localStorage.getItem("dataNote");
    if (data) {
      data = JSON.parse(data);
    }
    dataNote.value = data.filter((e) => {
      if (
        e.title.toLowerCase().search(newSearchText.toLowerCase()) > -1 ||
        e.note.toLowerCase().search(newSearchText.toLowerCase()) > -1
      ) {
        return true;
      }
    });
  } else {
    getDataNote();
  }
});

getDataNote();

function getDataNote() {
  dataNote.value = localStorage.getItem("dataNote");
  if (dataNote.value) {
    dataNote.value = JSON.parse(dataNote.value);
  }
}

function showEditModalFn(item) {
  itemEditModal.value = item;
  showEditModal.value = true;
}
</script>

<style scoped>
.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}
</style>
