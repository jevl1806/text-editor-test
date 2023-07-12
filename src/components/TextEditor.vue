<script setup lang="ts">
import { QuillEditor } from "@vueup/vue-quill";
import "@vueup/vue-quill/dist/vue-quill.snow.css";

interface Props {
  modelValue: string;
  placeholder?: string;
}

withDefaults(defineProps<Props>(), { placeholder: "Your text here..." });

const emit = defineEmits<(e: "update:modelValue", newValue: string) => void>();

const options = {
  modules: {
    toolbar: [
      [{ header: [1, 2, 3, 4, 5, 6, false] }],
      ["bold", "italic", "underline", "strike"],
      [
        {
          color: ["black", "red", "blue", "green", "yellow"],
        },
        {
          background: ["black", "red", "blue", "green", "yellow"],
        },
      ],
      [{ list: "ordered" }, { list: "bullet" }],
      [{ indent: "-1" }, { indent: "+1" }],
      [{ script: "sub" }, { script: "super" }],
    ],
  },
  formats: [
    "background",
    "bold",
    "color",
    "font",
    "italic",
    "size",
    "strike",
    "underline",
    "blockquote",
    "header",
    "indent",
    "list",
    "align",
    "direction",
    "formula",
  ],
};

const handleUpdateContent = (value: string) => {
  const emptyPattern = /^<(p|h\d)><br><\/(p|h\d)>$/;

  if (emptyPattern.test(value)) emit("update:modelValue", "");
  else emit("update:modelValue", value);
};
</script>
<template>
  <div
    class="min-h-[110px] flex flex-col text-editor border border-[#999999] border-solid rounded-xl"
  >
    <QuillEditor
      theme="snow"
      :options="options"
      :content="modelValue"
      content-type="html"
      :placeholder="placeholder"
      @update:content="handleUpdateContent"
    />
  </div>
</template>

<style lang="postcss">
.text-editor .ql-toolbar.ql-snow {
  @apply border-t-0 border-l-0 border-r-0 rounded-t-xl;
}

.ql-snow .ql-picker-label::before,
.ql-snow .ql-picker-options .ql-picker-item {
  color: #3b3679;
}

.text-editor .ql-container.ql-snow {
  @apply border-b-0 border-l-0 border-r-0 rounded-b-xl;
}

.text-editor .ql-formats svg .ql-fill {
  @apply fill-[#3B3679];
}

.text-editor .ql-formats svg .ql-stroke {
  @apply stroke-[#3B3679];
}

.text-editor .ql-editor.ql-blank::before {
  @apply text-[#8E8E8E] not-italic text-base font-normal;
}
</style>
