<template>
  <div style="width:100%; height: 100%; background-color: yellow">
    <p>above</p>
    <VueDiagram
        style="background-color: red; height: 500px;width:500px;"
        divClassName='gojs-diagram'
        :initDiagram="initDiagram"
        :nodeDataArray="nodeDataArray"
        :linkDataArray="linkDataArray"
        :modelData="modelData"
        @modelChange="handleModelChange"
    ></VueDiagram>
    <p>below</p>
    <button @click="appendNode">more</button>
  </div>
</template>

<script>
import {VueDiagram} from 'gojs-vue';
import * as go from 'gojs';

export default {
  name: 'HelloWorld',
  components: {
    VueDiagram
  },
  data() {
    return {
      modelData: {
        canRelink: true
      },
      linkDataArray: [
        {key: 1, from: 'Alpha', to: 'Beta'},
        {key: 2, from: 'Alpha', to: 'Gamma'},
        {key: 3, from: 'Beta', to: 'Beta'},
        {key: 4, from: 'Gamma', to: 'Delta'},
        {key: 5, from: 'Delta', to: 'Alpha'}
      ],
      nodeDataArray: [
        {key: 'Alpha', color: 'lightblue'},
        {key: 'Beta', color: 'orange'},
        {key: 'Gamma', color: 'lightgreen'},
        {key: 'Delta', color: 'pink'}
      ]
    }
  },
  props: {
    msg: String
  },
  methods: {
    appendNode() {
      this.nodeDataArray.push({key: `${Math.random() * 10}`, color: 'blue'});
      console.log('pushed');
    },
    initDiagram() {
      const $ = go.GraphObject.make
      const diagram = $(go.Diagram, {
        'undoManager.isEnabled': true,
        model: $(go.GraphLinksModel, {
          linkKeyProperty: 'key'
        })
      })
      diagram.nodeTemplate = $(go.Node, 'Auto',
          $(go.Shape, 'RoundedRectangle', {
                strokeWidth: 0,
                fill: 'white',
                portId: '',
                fromLinkable: true,
                toLinkable: true,
                cursor: 'pointer'
              },
              new go.Binding('fill', 'color')),
          $(go.TextBlock, {
                margin: 8,
                font: 'bold 14px sans-serif',
                stroke: '#333'
              },
              new go.Binding('text', 'key'))
      )

      diagram.linkTemplate = $(go.Link,
          new go.Binding('relinkableFrom', 'canRelink').ofModel(),
          new go.Binding('relinkableTo', 'canRelink').ofModel(),
          $(go.Shape),
          $(go.Shape, {toArrow: 'Standard'})
      )
      return diagram
    },
    handleModelChange(data) {
      console.log({data});
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.gojs-diagram {
  position: relative;
  width: 100%;
  height: 100%;
}

h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
