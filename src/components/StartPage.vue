<template>
  <div class="start-page">
    <form id="form">
      <input type="text" placeholder="Input your text" id="text-input" v-model="newRow" />
      <br />
      <button class="addButton" v-on:click="AddElement">Add</button>
    </form>
    <form id="sort-form">
      <p id="text-line">Sort by:</p>
      <input type="radio" class="asc" name="sort-radiobutton"  id="number-sort" @click="sortParams.field='id', sortParams.order='asc'" checked />
      <label for="number-sort">Number</label>
      <input type="radio" class="desc" name="sort-radiobutton" id="rnumber-sort" @click="sortParams.field='id', sortParams.order='desc'" />
      <label for="rnumber-sort">Reverse number</label>
      <input type="radio" class="asc" name="sort-radiobutton" id="alphabet-sort" @click="sortParams.field='value', sortParams.order='asc'" />
      <label for="alphabet-sort">Alphabet</label>
      <input type="radio" class="desc" name="sort-radiobutton" id="ralphabet-sort" @click="sortParams.field='value', sortParams.order='desc'" />
      <label for="ralphabet-sort">Reverse alphabet</label>
    </form>
    <div class="ul-list">
      <p>{{msg}}</p>
      <ul id="list-item">
        <li v-for="(item, index) in sortedList" :key="item.id">
          <template v-if="editIndex !== index">
            <a>{{item.id}}. {{item.value}}</a>
             <button class="operationButton" v-on:click="DeleteElement(index)" >Delete</button>
            <button class="operationButton" v-on:click="EditElement(index)">Edit</button>
          </template>
          <template v-else>
            <a>{{item.id}}. </a>
            <input type="text" class="edit-textinput" :value="item.value" />
            <button class="submitButton" v-on:click="editIndex = -1">Cancel</button>
            <button class="submitButton" v-on:click="SubmitChange(index)">Ok</button>
          </template>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'StartPage',
  data () {
    return {
      msg: 'Your list:',
      editIndex: -1,
      sortParams: {
        field: 'id',
        order: 'asc'
      },
      newRow: '',
      list: []
    }
  },
  methods: {
    AddElement () {
      if (this.newRow) {
        let elem = {}
        elem['id'] = this.computedId
        elem['value'] = this.newRow
        this.list.push(elem)
      }
      this.newRow = ''
    },
    EditElement (index) {
      this.editIndex = index
    },
    SubmitChange (index) {
      this.list = this.sortedList
      let editedRow = document.querySelector('.edit-textinput').value
      if (editedRow) this.list[index].value = editedRow
      this.editIndex = -1
    },
    DeleteElement (index) {
      this.list = this.sortedList
      this.list.splice(index, 1)
    }
  },
  computed: {
    computedId: function () {
      let length = this.list.length
      if (length === 0) return 1
      return this.list[length - 1].id + 1
    },
    sortedList: function () {
      let cloneList = this.list.slice()
      let field = this.sortParams.field
      if (!cloneList) return []
      cloneList.sort(function (nodeA, nodeB) {
        return (nodeA[field] > nodeB[field]) ? 1 : -1
      })
      return (this.sortParams.order === 'desc') ? cloneList.reverse() : cloneList
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.addButton {
  background: #ff00ff;
  border-radius: 10px;
  padding: 0.7em 1.5em;
  outline: none;
  margin-bottom: 20px;
  margin-top: 20px;
  width: 150px;
  font-size: 14pt;
}
#form,
#sort-form {
  background: #777777;
  border: 3px solid #000;
  border-radius: 10px;
  width: 500px;
  margin: 0 auto;
  margin-bottom: 20px;
}
#text-line {
  font-size: 14pt;
  text-align: center;
}
#text-input {
  background: #dcdcdc;
  border-radius: 10px;
  margin-bottom: 20px;
  margin-top: 20px;
  height: 50px;
  width: 250px;
  font-size: 14pt;
}

#list-item,
.edit-textinput {
  font-size: 16pt;
}
.operationButton {
  background: #fff;
  outline: none;
  font-size: 16pt;
  border: 0;
}
.submitButton {
  background: #fff;
  outline: none;
  font-size: 16pt;
  border: 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin: 0 10px;
}
a, p {
  color: #42b983;
  margin-left: 20px;
  margin-right: 20px;
}
</style>
