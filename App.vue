<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>{{ msg }}</h1>
    <h2>Tree View</h2>
    <div>
      <div style="width:840px; margin: 0 auto;">
        <div style="width:49%; display:inline-block; vertical-align: top;">
          <p style="text-align:left">Search Text <input type="text" @keyup="inputKeyUp" v-model="searchText" /></p>
          <br>
          <v-jstree :data="data" text-field-name="text" :item-events="itemEvents" show-checkbox multiple allow-batch whole-row draggable @item-click="itemClick" ref="tree"></v-jstree>
        </div>
        <div style="width:50%; display:inline-block;">
        <textarea  style="height:300px; width:100%;">
          {{data}}
        </textarea>
        </div>
      </div>
    </div>
    <h2>Edit Tree Item</h2>
    <p>click the node for edit</p>
    <div>
      <div style="width:840px; height:300px; margin: 0 auto;">
        <table>
          <tr>
            <td>
              text
            </td>
            <td>
              <input v-model="editingItem.text" />
            </td>
          </tr>
          <tr>
            <td>
              value
            </td>
            <td>
              <input v-model="editingItem.value" />
            </td>
          </tr>
          <tr>
            <td>
              icon
            </td>
            <td>
              <input v-model="editingItem.icon" />
            </td>
          </tr>
          <tr>
            <td>
              opened
            </td>
            <td>
              <input type="checkbox" v-model="editingItem.opened"/>
            </td>
          </tr>
          <tr>
            <td>
              selected
            </td>
            <td>
              <input type="checkbox" v-model="editingItem.selected" />
            </td>
          </tr>
          <tr>
            <td>
              disabled
            </td>
            <td>
              <input type="checkbox" v-model="editingItem.disabled" />
            </td>
          </tr>
          <tr>
            <td colspan="2">
              <button @click="addChildNode">add child node</button>
              <button @click="removeNode">remove this node</button>
              <button @click="addBeforeNode">add child before node</button>
              <button @click="addAfterNode">add child after node</button>
              <button @click="openChildren">open child node</button>
              <button @click="closeChildren">close child node</button>
            </td>
          </tr>
        </table>
      </div>
    </div>
    <h2>Async Loading</h2>
    <div>
      <div style="width:840px; margin: 0 auto;">
        <div style="width:49%; display:inline-block; vertical-align: top;">
          <v-jstree :data="asyncData" :async="loadData" show-checkbox multiple allow-batch whole-row @item-click="itemClick"></v-jstree>
        </div>
        <div style="width:50%; display:inline-block; vertical-align: top;">
        <textarea  style="height:300px; width:100%;">
          {{asyncData}}
        </textarea>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'app',
    data () {
      return {
        msg: 'A Tree Plugin For Vue2',
        searchText: '',
        editingItem: {},
        editingNode: null,
        itemEvents: {
          mouseover: function () {
            console.log('mouseover')
          },
          contextmenu: function () {
            console.log(arguments[2])
            arguments[2].preventDefault()
            console.log('contextmenu')
          }
        },
        data: [
          {
            "text": "Same but with checkboxes",
            "children": [
              {
                "text": "initially selected",
                "selected": true
              },
              {
                "text": "custom icon",
                "icon": "fa fa-warning icon-state-danger"
              },
              {
                "text": "initially open",
                "icon": "fa fa-folder icon-state-default",
                "opened": true,
                "children": [
                  {
                    "text": "Another node"
                  }
                ]
              },
              {
                "text": "custom icon",
                "icon": "fa fa-warning icon-state-warning"
              },
              {
                "text": "disabled node",
                "icon": "fa fa-check icon-state-success",
                "disabled": true
              }
            ]
          },
          {
            "text": "Same but with checkboxes",
            "opened": true,
            "children": [
              {
                "text": "initially selected",
                "selected": true
              },
              {
                "text": "custom icon",
                "icon": "fa fa-warning icon-state-danger"
              },
              {
                "text": "initially open",
                "icon": "fa fa-folder icon-state-default",
                "opened": true,
                "children": [
                  {
                    "text": "Another node"
                  }
                ]
              },
              {
                "text": "custom icon",
                "icon": "fa fa-warning icon-state-warning"
              },
              {
                "text": "disabled node",
                "icon": "fa fa-check icon-state-success",
                "disabled": true
              }
            ]
          },
          {
            "text": "And wholerow selection"
          }
        ],
        data2: [
          {
            "text2": "Same but with checkboxes",
            "children": [
              {
                "text2": "initially selected",
                "selected": true
              },
              {
                "text2": "custom icon",
                "icon": "fa fa-warning icon-state-danger"
              },
              {
                "text2": "initially open",
                "icon": "fa fa-folder icon-state-default",
                "opened": true,
                "children": [
                  {
                    "text2": "Another node"
                  }
                ]
              },
              {
                "text2": "custom icon",
                "icon": "fa fa-warning icon-state-warning"
              },
              {
                "text2": "disabled node",
                "icon": "fa fa-check icon-state-success",
                "disabled": true
              }
            ]
          },
          {
            "text2": "Same but with checkboxes",
            "opened": true,
            "children": [
              {
                "text2": "initially selected",
                "selected": true
              },
              {
                "text2": "custom icon",
                "icon": "fa fa-warning icon-state-danger"
              },
              {
                "text2": "initially open",
                "icon": "fa fa-folder icon-state-default",
                "opened": true,
                "children": [
                  {
                    "text2": "Another node"
                  }
                ]
              },
              {
                "text2": "custom icon",
                "icon": "fa fa-warning icon-state-warning"
              },
              {
                "text2": "disabled node",
                "icon": "fa fa-check icon-state-success",
                "disabled": true
              }
            ]
          },
          {
            "text2": "And wholerow selection"
          }
        ],
        asyncData: [],
        loadData: (oriNode, resolve) => {
          var id = oriNode.data.id ? oriNode.data.id : 0
          setTimeout(() => {
            let data = []
            if (id > 20) {
              data = []
            }
            else {
              data = [
                {
                  "text": "New Item 1..." + id
                },
                {
                  "text": "New Item 2..." + id
                }
              ]
            }
            resolve(data)
          }, 500)
        }
      }
    },
    methods: {
      itemClick (node) {
        this.editingNode = node
        this.editingItem = node.model
        console.log(node.model.text + ' clicked !')
      },
      inputKeyUp: function () {
        var text = this.searchText
        const patt = new RegExp(text);
        this.$refs.tree.handleRecursionNodeChilds(this.$refs.tree, function (node) {
          if (text !== '') {
            const str = node.model.text
            if (patt.test(str)) {
              node.$el.querySelector('.tree-anchor').style.color = 'red'
            } else {
              node.$el.querySelector('.tree-anchor').style.color = '#000'
            } // or other operations
          } else {
            node.$el.querySelector('.tree-anchor').style.color = '#000'
          }
        })
      },
      addChildNode: function () {
        if (this.editingItem.id !== undefined) {
          this.editingItem.addChild({
            text: "newNode",
            value: "newNode"
          })
        }
      },
      removeNode: function () {
        if (this.editingItem.id !== undefined) {
          var index = this.editingNode.parentItem.indexOf(this.editingItem)
          this.editingNode.parentItem.splice(index, 1)
        }
      },
      addBeforeNode: function () {
        if (this.editingItem.id !== undefined) {
          this.editingItem.addBefore({
            text: "newNode",
            value: "newNode"
          }, this.editingNode)
        }
      },
      addAfterNode: function () {
        if (this.editingItem.id !== undefined) {
          this.editingItem.addAfter({
            text: "newNode",
            value: "newNode"
          }, this.editingNode)
        }
      },
      openChildren: function () {
        if (this.editingItem.id !== undefined) {
          this.editingItem.openChildren()
        }
      },
      closeChildren: function () {
        if (this.editingItem.id !== undefined) {
          this.editingItem.closeChildren()
        }
      }
    }
  }
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
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

div{
  display: block;
}

table {
  width: 100%;
  border-collapse: collapse;
  border: 1px solid #EEE;
  font-size: 14px;
}

table th {
  background: #EEE;
  border-bottom: 1px solid #CCC;
  padding: 4px;
}

table td {
  border: 1px solid #EEE;
  padding: 4px;
}
</style>
