<template>
<div id="app">
         <ejs-treegrid ref='treegrid' :dataSource='data' childMapping='subtasks' :treeColumnIndex='1' :height='380' :editSettings='editSettings' :dataBound='dataBound' :expanded='expanded' :collapsed='collapsed'>
            <e-columns>
                <e-column field='taskID' headerText='Task ID' isPrimaryKey='true' width='70' textAlign='Right'></e-column>
                <e-column field='taskName' headerText='Task Name' width='200'></e-column>
                <e-column field='startDate' headerText='Start Date' width='90' format="yMd" textAlign='Right'></e-column>
                <e-column field='endDate' headerText='End Date' width='90' format="yMd" textAlign='Right'></e-column>
                <e-column field='duration' headerText='Duration' width='80' textAlign='Right'></e-column>
                <e-column field='progress' headerText='Progress' width='80' textAlign='Right'></e-column>
                <e-column field='priority' headerText='Priority' width='90'></e-column>
            </e-columns>
        </ejs-treegrid>
</div>
</template>
<script>
import Vue from "vue";
import { TreeGridPlugin, Page, Edit } from "@syncfusion/ej2-vue-treegrid";
import { sampleData } from "./datasource.js";
Vue.use(TreeGridPlugin);

export default {
  data ()  {
    return {
      currentData : [],
      data: sampleData,
      pageSettings: { pageSize: 7 },
      editSettings: { allowEditing: true, allowAdding: true, allowDeleting: true, mode:"Row" },
    };
  },
  provide: {
      treegrid: [ Page, Edit ]
  },
  methods:{
    dataBound:function (args) {
        // if (this.$refs.treegrid.initialRender) {
      //checking whether it is initial rendering
      var data = JSON.parse(window.localStorage.getItem('currentData'));
      //retriving collapsed record in local storage using getItem method
      if (data) {
        var completeData = this.$refs.treegrid.ej2Instances.grid.dataSource;
        var primaryKeyFieldName = this.$refs.treegrid.getPrimaryKeyFieldNames()[0];
        for (var i = 0; i < data.length; i++) {
          var value;
          for (var j = 0; j < completeData.length; j++) {
            if (
              completeData[j][primaryKeyFieldName] ==
              data[i][primaryKeyFieldName]
            ) {
              value = completeData[j];
            }
          }
          this.$refs.treegrid.collapseRow(null, value); //collapsing row using collapseRow method
        }
        this.currentData = [];
        window.localStorage.setItem('currentData',JSON.stringify(this.currentData));
      }
    },
    
    expanded:function (args) {
      this.currentData.push(args.data);
      this.store(args);
    },
    collapsed:function (args) {
      this.currentData.push(args.data);
      this.store(args);
    },
    store:function (args) {
       this.currentData.push(...this.$refs.treegrid.getCurrentViewRecords().filter((i) => i.hasChildRecords).filter((i) => !i.expanded));
    this.currentData = Array.from(new Set(this.currentData));
    window.localStorage.setItem('currentData',JSON.stringify(this.currentData));
    }
  }
  }
</script>


        
    

<style>
@import '../node_modules/@syncfusion/ej2-base/styles/material.css';  
@import '../node_modules/@syncfusion/ej2-buttons/styles/material.css';  
@import '../node_modules/@syncfusion/ej2-calendars/styles/material.css';  
@import '../node_modules/@syncfusion/ej2-dropdowns/styles/material.css';  
@import '../node_modules/@syncfusion/ej2-inputs/styles/material.css';  
@import '../node_modules/@syncfusion/ej2-navigations/styles/material.css';
@import '../node_modules/@syncfusion/ej2-popups/styles/material.css';
@import '../node_modules/@syncfusion/ej2-splitbuttons/styles/material.css';
@import "../node_modules/@syncfusion/ej2-grids/styles/material.css";
@import "../node_modules/@syncfusion/ej2-treegrid/styles/material.css";
</style>