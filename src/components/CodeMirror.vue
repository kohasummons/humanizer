<template>
  <div id="editor"></div>
</template>

<script setup>
import { defineComponent, ref, computed, onMounted } from "vue";
import { EditorView, basicSetup } from "codemirror";
import { EditorState, Compartment } from "@codemirror/state";
import { oneDark } from "@codemirror/theme-one-dark";
import { javascriptLanguage } from "@codemirror/lang-javascript";
import { LanguageSupport } from "@codemirror/language";
import { utils } from "ethers";

const props = defineProps({
  formatType: String,
});

const format = computed(() => props.formatType);

const boiler = `🔧 Humanizer

// How to Use
// - Replace editor content with your Json abi
// - Select your format type
//   -> Full adds the name of the arguments (recommended)
//   -> Minimal strips off argument names leaving only their data type
// - Generate
`;

let languageExtensions = {
  javascript: [new LanguageSupport(javascriptLanguage)],
};

defineComponent({
  name: "codeMirror",
});

// const content = ref("");
const editor = ref("");
const view = ref();
const x = computed(() => editor.value);

onMounted(() => {
  const state = EditorState.create({
    doc: boiler,
    extensions: [
      EditorView.updateListener.of((e) => {
        editor.value = e.state.doc.toString();
      }),
      basicSetup,
      oneDark,
      ...languageExtensions["javascript"],
    ],
  });

  view.value = new EditorView({
    state,
    parent: document.querySelector("#editor"),
    lineWrapping: true,
  });
});

function handleCopy() {
  const docToCopy = x.value.toString("utf-8");
  navigator.clipboard.writeText(docToCopy);
  alert("Humanizer: Copied");
}

function handleAbiGenerator() {
  try {
    const parsedAbi = JSON.parse(x.value.toString("utf-8"));
    const contractName = parsedAbi.contractName + "ABI";

    const contractABI = new utils.Interface(parsedAbi.abi);
    const HumaneABI = contractABI.format(format.value);
    const formattedHumaneABI = JSON.stringify(HumaneABI, null, 4);
    const abi = `export const ${contractName} =  ${formattedHumaneABI};\n\n`;

    console.log(HumaneABI);

    replaceEditorContent(abi);
  } catch (error) {
    alert(error);
  }
}

function replaceEditorContent(value) {
  const doc = view.value.state.doc;
  view.value.destroy();
  view.value = new EditorView({
    state: EditorState.create({
      doc: value,
      extensions: [
        EditorView.updateListener.of((e) => {
          editor.value = e.state.doc.toString();
        }),
        basicSetup,
        oneDark,
        ...languageExtensions["javascript"],
      ],
    }),
    parent: document.querySelector("#editor"),
  });
}

// function replaceEditorContent(humaneABI) {
//   console.log(view.value.state.doc.length);

//   const toggleLang = EditorState.transactionExtender.of((tr) => {
//     return {
//       effects: language.reconfigure(javascript()),
//     };
//   });

//   const newState = EditorState.create({
//     doc: humaneABI,
//     extensions: [
//       basicSetup,
//       oneDark,
//       javascript(),
//       EditorView.updateListener.of((e) => {
//         editor.value = e.state.doc.toString();
//       }),
//     ],
//   });

//   view.value.setState(newState);
// }

defineExpose({
  replaceEditorContent,
  handleAbiGenerator,
  handleCopy,
});
</script>

<style>
#editor {
  /* height: fit-content; */
  width: auto;
}

.cm-content,
.cm-gutter {
  max-height: 300px;
}

.cm-editor {
  height: 300px;
}

.cm-gutters {
  margin: 1px;
}

.cm-scroller {
  overflow: auto;
}

.cm-wrap {
  border: 1px solid silver;
}

@media screen and (min-width: 768px) {
  #editor {
    /* height: fit-content; */
    width: 950px;
  }

  .cm-content,
  .cm-gutter {
    height: 80vh;
  }

  .cm-editor {
    height: 80vh;
  }
}
</style>