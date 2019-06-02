<template>
  <div class="json-schema"></div>
</template>

<script>
import JSONSchemaView from "json-schema-view-js"

export default {
  name: "JsonSchema",
  props: {
    levels: {
      type: Number,
      default: 1
    },
    schema: {
      type: Object,
      default: () => ({})
    }
  },
  mounted() {
    this.$el.appendChild(new JSONSchemaView(this.schema, this.levels).render())
  }
}
</script>

<style scoped lang="scss">
.json-schema {
  padding: 15px;
  background: #f4f4f4;
  display: block;
  margin: 60px 0;
  border-radius: 4px;
  overflow: auto;

  & /deep/ .json-schema-view {
    font-family: monospace;
    font-size: 0;
    display: table-cell;

    & > * {
      font-size: 14px;
      font-family: Menlo, Monaco, Consolas, "Courier New", monospace;
    }
    .toggle-handle {
      cursor: pointer;
      margin: auto 0.3em;
      display: inline-block;
      transform-origin: 50% 50%;
      transition: transform 150ms ease-in;
    }
    .toggle-handle:after {
      content: "\25BC";
    }
    .toggle-handle,
    .toggle-handle:hover {
      text-decoration: none;
      color: #333;
    }
    .description {
      color: gray;
      font-style: italic;
    }
    .title {
      font-weight: bold;
      cursor: pointer;
    }
    .title,
    .title:hover {
      text-decoration: none;
      color: #333;
    }
    .title,
    .brace,
    .bracket {
      color: #333;
    }
    .property {
      font-size: 0;
      display: table-row;
    }
    .property > * {
      font-size: 14px;
      padding: 0.2em;
    }
    .name {
      color: #007dc1;
      display: table-cell;
      vertical-align: top;
    }
    .type {
      color: #2aa198;
    }
    .type-any {
      color: #e22866;
    }
    .required {
      color: #f00;
    }
    .format
    .enums,
    .pattern {
      color: #000;
    }
    .inner {
      padding-left: 18px;
    }
    .json-formatter-number,
    .json-formatter-bracket,
    .json-formatter-key {
      color: #00659d;
    }
    .json-formatter-string {
      color: #009900;
    }
    &.collapsed .description {
      display: none;
    }
    &.collapsed .property {
      display: none;
    }
    &.collapsed .closeing.brace {
      display: inline-block;
    }
    &.collapsed .toggle-handle {
      transform: rotate(-90deg);
    }
  }
}
</style>