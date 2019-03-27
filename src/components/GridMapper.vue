<template>
  <div>
    <ejs-textbox v-bind:value="cellvalue"></ejs-textbox>
  </div>
</template>
<script lang="ts">
import { Vue, Component, Prop, Provide } from "vue-property-decorator";

@Component
export default class GridMapper extends Vue {
  @Prop() public column!: any;
  @Prop() public initValue: any = null;

  private componentName: string = "- not initialized -";
  private cellvalue: string = "- not initialized -";

  public constructor(options: any) {
    super(options);
    if (options) {
      if (options.column) {
        this.column = options.column;
      }
      if (options.initValue) {
        this.initValue = options.initValue;
      }
    }
  }

  public created() {
    console.log("gridmapper created");
    this.cellvalue = "created";
    }

  public mounted() {
    console.log("gridmapper mounted");
    if (this.column) {
      this.componentName = this.column.title;
    }
  }

  public setValue(val: any) {
    console.log("gridmapper setValue");
    this.cellvalue = val;
  }

  private valueChanged(val: any) {
    this.$emit("input", this.cellvalue);
  }
}
</script>
