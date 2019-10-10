<template>
  <div id="holder">
    <!-- {{elements}} -->
    <cytoscape
      :config="config"
      :preConfig="preConfig"
      :afterCreated="afterCreated"
    >
      <cy-element
        v-for="def in elements.nodes"
        :key="`${def.data.id}`"
        :definition="def"
      />
      <cy-element
        v-for="def in elements.edges"
        :key="`${def.data.id}`"
        :definition="def"
      />
    </cytoscape>
  </div>
</template>

<script>
/* eslint-disable */
import cola from "cytoscape-cola";
import elements from "./assets/test.json";
import axios from "axios";
// import cyspringy from 'cytoscape-springy'
const config = {
  autounselectify: false,
  boxSelectionEnabled: true,
  // zoom:1,
  // maxZoom:1e50,
  layout: {
    name: "cola"
  },
  style: [
    {
      selector: "node",
      css: {
        'label':'data(name)',
        // 'source-label':'data(name)',
        // 'target-label':'data(name)',
        "background-color": "red",
        // "width": "mapData(degree, 0, 20, 20, 60)",
        // "height": "mapData(degree, 0, 20, 20, 60)",
        // 'shape':'star',
        // 'border-style':'dotted',
        "text-valign" : "bottom",
        "text-halign" : "center",
        "font-size":'15',
        'text-wrap': 'wrap',
        // 'text-max-width':'1px',
        // 'text-overflow-wrap':'\n'
        // 'curve-style': 'taxi'

      }
    },
    {
      selector: "node[type='comp']",
      css: {
        label:'data(name)',
        "background-color": 'darkcyan', 
        "width": '45',
        "height": '45',
        // 'shape':'star',
        // 'border-style':'dotted',
        "text-valign" : "bottom",
        "text-halign" : "center",
        'text-wrap': 'wrap'

      }
    },
    {
      selector: "edge",
      css: {
        label:'data(type)',
        "line-color": "coral",
        'width':1,
        // "curve-style":'taxi'

      }
    }
  ],
  autolock:false,
  autoungrabify:false,
  elements: []
};
// const elements = elements
export default {
  name: "app",
  data() {
    return {
      config,
      elements,
      i: 1
    };
  },
  methods: {
    preConfig(cytoscape) {
      console.log("calling pre-config", config, elements);
      // cytoscape: this is the cytoscape constructor
      cytoscape.use(cola);
      // cyspringy(cytoscape)
    },
    async afterCreated() {
      console.log("after created");
      const cy = await this.$cytoscape.instance;
      /*cy.layout({
        name: 'breadthfirst',
        circle: false,
        roots: '#existingnode',
        spacingFactor: 1,
        padding: 50,
        fit: true,
      }).run();*/
      cy.elements()
        .layout({ name: "cola" ,
                  // nodeDimensionsIncludeLabels:false,
                  // convergenceThreshold: 100
                  // nodeSpacing:function(node){ return 20; },
                  infinite:true,
                  // fit:'true',
                  // avoidOverlap:false
                })
        .run();
        console.log(cy)
      // cy.fit()
      console.log(this.$cytoscape)
    },
    remove(cytoscape){

    }
  }
};
</script>

<style>
#holder {
  width: 100%;
  height: 1000px;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
