<template>
  <div id="holder">
    <cytoscape 
    :key=cyKey()
    :config="config" 
    :preConfig="preConfig" 
    :afterCreated="afterCreated"/>
    <cy-element
        v-for="def in config.elements"
        :key="`${def.data.id}`"
        :definition="def"
      />
  </div>
</template>

<script>
import Cytoscape from './components/Cytoscape'
import config from '@/utils/dummy-config'
import CyObj from '@/components/cy-object'
import jquery from 'jquery'
import contextMenus from 'cytoscape-context-menus'
import 'cytoscape-context-menus/cytoscape-context-menus.css'

export default {
  name: 'App',
  data () {
    return {
      config: config,
      i: 0
    }
  },
  methods: {
    preConfig (cytoscape) {
      console.log('calling pre-config')
      contextMenus(cytoscape, jquery)
      // cytoscape.use(contextMenus, jquery)
    },
    afterCreated (cy) {
      console.log('after created')
      // demo your core ext
      /*      {
        id: 'remove', // ID of menu item
        content: 'remove', // Display content of menu item
        tooltipText: 'remove', // Tooltip text for menu item
        image: {src : "remove.svg", width : 12, height : 12, x : 6, y : 4}, // menu icon
        // Filters the elements to have this menu item on cxttap
        // If the selector is not truthy no elements will have this menu item on cxttap
        selector: 'node, edge', 
        onClickFunction: function () { // The function to be executed on click
          console.log('remove element');
        },
        disabled: false, // Whether the item will be created as disabled
        show: false, // Whether the item will be shown or not
        hasTrailingDivider: true, // Whether the item will have a trailing divider
        coreAsWell: false // Whether core instance have this item on cxttap
      },
      {
        id: 'hide',
        content: 'hide',
        tooltipText: 'hide',
        selector: 'node, edge',
        onClickFunction: function () {
          console.log('hide element');
        },
        disabled: true
      },
      {
        id: 'add-node',
        content: 'add node',
        tooltipText: 'add node',
        image: {src : "add.svg", width : 12, height : 12, x : 6, y : 4},
        selector: 'node',
        coreAsWell: true,
        onClickFunction: function () {
          console.log('add node');
        }
      }*/
      cy.contextMenus({
        menuItems: [
          {
            id: 'remove',
            content: 'remove',
            tooltipText: 'remove',
            image: {src: 'remove.svg', width: 12, height: 12, x: 6, y: 4},
            selector: 'node, edge',
            onClickFunction: function (event) {
              var target = event.target || event.cyTarget
              target.remove()
            },
            hasTrailingDivider: true
          },
          {
            id: 'hide',
            content: 'hide',
            tooltipText: 'hide',
            selector: '*',
            onClickFunction: function (event) {
              var target = event.target || event.cyTarget
              target.hide()
            },
            disabled: true
          },
          {
          id: 'add-node',
          content: 'add node',
          tooltipText: 'add node',
          image: {src : "add.svg", width : 12, height : 12, x : 6, y : 4},
          selector: 'node',
          coreAsWell: true,
          onClickFunction: event => {
              // console.log(i)
              const { position } = event
              const n = {
                group: 'nodes',
                data: { id: `n` },
                position
              }
              this.config.elements.push(n)
            console.log('add node');
            }
          }
        ]
      })
    },
    cyKey () {
      const that = this
      CyObj.reset()
      CyObj.instance.then(cy => {
        console.log('cy', cy)
        cy.on('tap', event => {
          console.log('tapped')
          that.i++
        })
      })
      console.log('computing cyKey cy' + this.i)
      return 'cy' + this.i
    }
  },
  components: {
    Cytoscape
  }
}
</script>

<style>
#holder {
  width: 100%;
  height: 400px;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
