<template>
  <div
    class="editor"
    @keydown.prevent="handleKeyboardPress"
    tabindex="0"
    data-test="editor"
  >{{documentData}}</div>
</template>

<script>
const getKeyValueToRender = (keyCode, keyValue) => {
  switch (keyCode) {
    // Enter
    case 13:
      return '\n';
    // Tab
    case 9:
      return '\xa0\xa0';
    default:
      return keyValue;
  }
};

const getDefaultValue = () => {
  if (window && window.localStorage) {
    return window.localStorage.getItem('documentData') || '';
  }
  return '';
};

const setValueToStorage = (value) => {
  if (window && window.localStorage) {
    window.localStorage.setItem('documentData', value);
  }
};

export default {
  name: 'Editor',
  data: () => ({
    documentData: getDefaultValue(),
  }),
  // define methods under the `methods` object
  methods: {
    handleKeyboardPress(event) {
      // handle key actions
      // backspace
      if (event.keyCode === 8) {
        this.documentData = this.documentData.slice(0, -1);
      } else {
        // handle key input
        this.documentData =
          this.documentData + getKeyValueToRender(event.keyCode, event.key);
      }
      setValueToStorage(this.documentData);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang='scss' scoped>
.editor {
  background-color: $editor-background;
  color: $editor-fontColor;
  height: 100%;
  white-space: pre;
  font-size: 16px;
  text-align: left;
  padding-left: 120px;
  overflow: scroll;
  :focus {
    outline: 0;
  }
}
</style>