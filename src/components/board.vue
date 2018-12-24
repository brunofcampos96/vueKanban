<template>
  <div id='board' v-bind:style="{background: this.color}"  v-if="(this.title || this.editing && !this.title) || this.supportBoard">
    <div class='content' v-if="!this.supportBoard">
      <div class='header' v-if="this.localEditing">
        <div class='title_div'>
          <b-form-input
            type='text'
            ref="titleInput"
            placeholder='Título'
            v-model="localTitle"
            @blur.native="cancelEditing()"
            @keyup.enter.native="cancelEditing()"
            class='font title_input'
            ></b-form-input>
        </div>
        <div class='icon' @click="saveTitle()">
          <a><font-awesome-icon size="lg" icon="save"/></a>
        </div>
      </div>
      <div class='header font' v-else-if="!this.localEditing" v-bind:style="{background: this.headColor}">
        <div class='title'>
          {{title}}
        </div>
        <div class='icon' @click="editTitle()">
          <font-awesome-icon size="lg" icon="pencil-alt"/>
        </div>
      </div>
      <div class='cards'>
        <Card 
          v-bind="card"
          :editing.sync="card.editing"
          :title.sync="card.title"
          v-for="card in cards" :key="card.id">
        </Card>
      </div>
      <div class='footer' v-bind:style="{background: this.headColor}">
        <div class='icon exclude' @click="deleteBoard(id)">
          <a><font-awesome-icon size="lg" icon="trash-alt"/></a>
        </div>
      </div>
    </div>
    <div id='supportBoard' v-else v-bind:style="{background: 'rgba(59, 230, 98, 0.72)'}">
      <b-button id='addButton' @click="addBoard()" variant="success">+</b-button>
    </div>
  </div>
</template>
<script>

import Card from './card.vue'
export default {
  data: () => ({
  }),
  methods: {
    cancelEditing () {
      this.localEditing = false
      if (!this.title) this.deleteBoard(this.id)
    },
    deleteBoard (bId) {
      this.$parent.deleteBoard(bId)
    },
    addBoard () {
      this.$parent.addBoard()
    },
    editTitle () {
      this.localEditing = true
    },
    saveTitle () {
      this.localEditing = false
    }
  },
  mounted () {
    if (this.localEditing) this.$refs.titleInput.focus()
  },
  updated () {
    if (this.localEditing) this.$refs.titleInput.focus()
  },
  computed: {
    localEditing: {
      get: function () {
        return this.editing
      },
      set: function (value) {
        this.$emit('update:editing', value)
      }
    },
    localTitle: {
      get: function () {
        return this.title
      },
      set: function (value) {
        this.$emit('update:title', value)
      }
    }
  },
  components: {
    Card
  },
  props: ['id', 'editing', 'title', 'cards', 'supportBoard', 'color', 'headColor']
}
</script>
<style scoped>
#board{
  display: table;
  height: 100%;
  overflow: auto;
  border: 2px solid rgb(204, 204, 204);
  border-radius: 5px;
}
#supportBoard{
  display: table-cell;
  vertical-align: middle;
}
#addButton{
  font-size: x-large;
  border-radius: 50%;
  width: 50px;
  height: 50px;
}
.header, .footer{
  height: 100%;
  display: grid;
  grid-template-columns: 20% 60% 20%;
  outline: none;
  border-top-left-radius: 5px;
  border-top-right-radius: 5px;
  word-break: break-word;
  align-items: center;
}
.footer{
  border-top-left-radius: 0px;
  border-top-right-radius: 0px;
  border-bottom-left-radius: 5px;
  border-bottom-right-radius: 5px;
}
.font{
  font-size: 1.3em;
  font-weight: bold;
  color: black;
  align-items: center;
}
.content{
  height: 100%;
  display: grid;
  width: 100%;
  grid-auto-rows: 10% 80% 10%;
}
.title{
  grid-column-start: 2;
  width: 100%;
  padding: 7px;
}
.icon{
  cursor: pointer;
  color: black;
}
.exclude{
  grid-column-start: 2;
}
.title_div{
  width: 100%;
  height: 100%;
  grid-column-end: 3;
  grid-column-start: 1;
}
.title_input{
  border-bottom: 0px;
  border-radius: 0px;
  border-top-left-radius: 5px;
  font-size: 1.3em;
  height: 100%;
  border: none;
  background: rgba(255, 255, 255, 0.308);
}
.cards{
  height: 100%;
  display: grid;
  padding: 5px;
  grid-template-columns: 1fr;
  grid-auto-flow: row;
  grid-auto-rows: max-content;
  grid-row-gap: 5px;
}
.center_img{
  margin-left: auto;
  margin-right: auto;
  display: block;
}
svg{
  max-width: 20px;
}
</style>
