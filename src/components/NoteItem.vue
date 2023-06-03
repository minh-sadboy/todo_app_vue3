<template>
  <div
    :class="[
      'note-item relative min-w-[380px] max-w-[600px] cursor-default rounded-md border-[1px] border-inherit bg-[#fff] px-[16px] py-[12px]',
      modeEdit ? 'h-[90vh]' : 'max-h-[380px]',
    ]"
  >
    <div
      class="icon-delete absolute left-[-10px] top-[-10px] hidden cursor-pointer"
      @click.stop="deleteNote"
      v-if="!modeEdit"
    >
      <IconDelete color="#fff" />
    </div>
    <div
      class="icon-push-pin absolute right-[16px] hidden cursor-pointer"
      @click.stop="pinNote"
      v-if="!modeEdit"
    >
      <IconPushPin />
    </div>
    <input
      type="text"
      class="mb-[8px] bg-[#fff] font-[600] outline-none"
      :value="title"
      :disabled="!modeEdit"
      v-if="!modeEdit"
    />
    <input
      type="text"
      class="mb-[8px] bg-[#fff] font-[600] outline-none"
      v-model="titleText"
      :disabled="!modeEdit"
      v-if="modeEdit"
    />
    <div
      class="max-h-[290px] overflow-hidden break-all text-[14px]"
      v-if="!modeEdit"
    >
      {{ textContent }}
    </div>
    <textarea
      :class="[
        'w-[100%] text-[14px] outline-none',
        modeEdit ? 'h-[calc(90vh-100px)]' : '',
      ]"
      rows="15"
      cols="50"
      v-model="textNote"
      v-if="modeEdit"
    ></textarea>
    <div
      class="float-right mr-[15px] mt-[5px] cursor-pointer text-[#0088ff]"
      v-if="modeCreate"
      @click="create"
    >
      Create
    </div>
    <div
      class="float-right mr-[15px] mt-[5px] cursor-pointer"
      v-if="modeEdit"
      @click="$emit('close')"
    >
      Close
    </div>
  </div>
</template>

<script setup>
import IconPushPin from "./icons/IconPushPin.vue";
import IconDelete from "./icons/IconDelete.vue";
import { ref, watch } from "vue";
const props = defineProps({
  id: Number,
  title: String,
  textContent: String,
  modeEdit: Boolean,
  modeCreate: Boolean,
});

const emit = defineEmits(["close", "create"]);

const textNote = ref(props.textContent);
const titleText = ref(props.title);

watch([textNote, titleText], ([newTextNote, newTitleText]) => {
  if (props.id) {
    let dataNote = localStorage.getItem("dataNote");
    if (dataNote) {
      dataNote = JSON.parse(dataNote);
    } else {
      dataNote = [];
    }

    let dataNoteIndex = -1;
    dataNote.forEach((e, i) => {
      if (e.id === props.id) dataNoteIndex = i;
    });
    if (dataNoteIndex > -1) {
      dataNote[dataNoteIndex].title = newTitleText
      dataNote[dataNoteIndex].note = newTextNote
    }
    localStorage.setItem("dataNote", JSON.stringify(dataNote));
    emit("create");
  }
});

function create() {
  const id = Date.now();
  let dataNote = localStorage.getItem("dataNote");
  if (dataNote) {
    dataNote = JSON.parse(dataNote);
  } else {
    dataNote = [];
  }
  dataNote.unshift({
    id: id,
    title: titleText.value,
    note: textNote.value,
  });
  localStorage.setItem("dataNote", JSON.stringify(dataNote));
  emit("close");
  emit("create");
}

function pinNote() {
  let dataNote = localStorage.getItem("dataNote");
  if (dataNote) {
    dataNote = JSON.parse(dataNote);
  } else {
    dataNote = [];
  }
  const noteIndex = dataNote.findIndex((e) => e.id === props.id);
  if (noteIndex > -1) {
    const note = dataNote[noteIndex];
    dataNote.splice(noteIndex, 1);
    dataNote.unshift(note);
  }
  localStorage.setItem("dataNote", JSON.stringify(dataNote));
  emit("create");
}

function deleteNote() {
  let dataNote = localStorage.getItem("dataNote");
  if (dataNote) {
    dataNote = JSON.parse(dataNote);
  } else {
    dataNote = [];
  }

  let dataNoteIndex = -1;
  dataNote.forEach((e, i) => {
    if (e.id === props.id) dataNoteIndex = i;
  });
  if (dataNoteIndex > -1) {
    dataNote.splice(dataNoteIndex, 1);
  }
  localStorage.setItem("dataNote", JSON.stringify(dataNote));
  emit("create");
}
</script>

<style scoped>
/* width */
textarea::-webkit-scrollbar {
  width: 10px;
}

/* Track */
textarea::-webkit-scrollbar-track {
  background: #fff;
}

/* Handle */
textarea::-webkit-scrollbar-thumb {
  background: #dad7d7;
}

/* Handle on hover */
textarea::-webkit-scrollbar-thumb:hover {
  background: #8d8c8c;
}

.note-item:hover .icon-push-pin,
.note-item:hover .icon-delete {
  display: block;
}
</style>
