<template>
  <div id="app">
    <HelloWorld msg="Welcome to my Vue.js + TypeScript + Syncfusion + Grid + Custom EditComponent App"/>

    <ejs-grid :dataSource="griddata" :editSettings="editSettings" :toolbar="toolbar" height="273px">
      <e-columns>
        <e-column
          field="OrderID"
          headerText="Order ID"
          textAlign="Right"
          :isPrimaryKey="true"
          width="100"
        ></e-column>
        <e-column field="CustomerID" headerText="Customer ID" width="120"></e-column>
        <e-column
          field="Freight"
          headerText="Freight"
          textAlign="Right"
          editType="numericedit"
          width="120"
          format="C2"
        ></e-column>
        <e-column
          field="OrderDate"
          headerText="Order Date"
          type="date"
          format="yMd"
          width="150"
          :edit="getEditCell({title: 'xx'})"
        ></e-column>
      </e-columns>
    </ejs-grid>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import HelloWorld from "./components/HelloWorld.vue";
import { TextBoxPlugin } from "@syncfusion/ej2-vue-inputs";

import {
  GridPlugin,
  Filter,
  Page,
  FilterType,
  GridComponent,
  Edit,
  Toolbar,
  ColumnChooser,
  ExcelExport,
  PdfExport,
  Resize,
  Sort,
  ColumnMenu,
  Group,
  Reorder
} from "@syncfusion/ej2-vue-grids";

import {
  setTemplateEngine,
  getTemplateEngine,
  getUniqueID,
  createElement,
  detach,
  extend
} from "@syncfusion/ej2-base";
import GridMapper from "@/components/GridMapper.vue";

Vue.use(TextBoxPlugin);
Vue.use(GridPlugin);

@Component({
  components: {
    HelloWorld,
    GridComponent
  },
  // dependency injection - das diese Subkomponenten zur Verfügung stehen
  provide: {
    grid: [
      Filter,
      ColumnChooser,
      Page,
      Edit,
      Toolbar,
      ExcelExport,
      PdfExport,
      Resize,
      Sort,
      Group,
      ColumnMenu,
      Reorder
    ]
  }
})
export default class App extends Vue {
  private griddata: any[] = [
    {
      OrderID: 1,
      CustomerID: 1,
      Freight: 2,
      OrderDate: new Date().toISOString()
    },
    {
      OrderID: 2,
      CustomerID: 1,
      Freight: 2,
      OrderDate: new Date().toISOString()
    },
    {
      OrderID: 3,
      CustomerID: 2,
      Freight: 2,
      OrderDate: new Date().toISOString()
    },
    {
      OrderID: 4,
      CustomerID: 2,
      Freight: 2,
      OrderDate: new Date().toISOString()
    }
  ];

  private editSettings: any = {
    allowEditing: true,
    allowAdding: true,
    allowDeleting: true,
    mode: "Normal"
  };
  private toolbar: any = ["Add", "Edit", "Delete", "Update", "Cancel"];

  private selectedRow: any = null;

  private getEditCell(column: any) {
    const pid = getUniqueID("templateParentDiv"); // create a emty div as container
    const iid = getUniqueID("templateParentDiv"); // create a emty div as container
    const ele = createElement("div", { id: pid });

    document.body.appendChild(ele); // append it somewhere

    // https://css-tricks.com/creating-vue-js-component-instances-programmatically/
    // prepare the vue Component for it
    const VueGridMapper = Vue.extend(GridMapper);
    let instance: any = null; // no current instance

    return {
      create(data: any) {
        console.log("template create()");
        return ele; // return the container element
      },
      read() {
        console.log("template read()");
        // return the value of the instance - but that gets never invoked!!
        return instance.getValue();
      },
      write(val: any) {
        console.log("template write()"); // mount the component to the container element
        instance = new VueGridMapper({
          propsData: { column }
        });
        instance.$mount(ele); // pass the container
        instance.setValue(val);
      },
      destroy() {
        try {
        console.log("template destroy()"); // destroy it again...
        instance.$destroy();
        instance = null;
        }catch(ex) {
          console.log("error destroying:", ex);
        }
      }
    };
  }
}
</script>

<style>
@import "../node_modules/@syncfusion/ej2-base/styles/material.css";
@import "../node_modules/@syncfusion/ej2-buttons/styles/material.css";
@import "../node_modules/@syncfusion/ej2-calendars/styles/material.css";
@import "../node_modules/@syncfusion/ej2-dropdowns/styles/material.css";
@import "../node_modules/@syncfusion/ej2-inputs/styles/material.css";
@import "../node_modules/@syncfusion/ej2-navigations/styles/material.css";
@import "../node_modules/@syncfusion/ej2-popups/styles/material.css";
@import "../node_modules/@syncfusion/ej2-splitbuttons/styles/material.css";
@import "../node_modules/@syncfusion/ej2-vue-grids/styles/material.css";

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
