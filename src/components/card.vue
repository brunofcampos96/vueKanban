<template>
    <div class='inner' @mouseout="cancelEditing()">
      <div class='header' :class="{collapsed_card: collapsed}" >
        <div class='arrow' @click="collapse()">
          <a>
            <font-awesome-icon 
            @click="cancelEditing()" :class="{rotate: !collapsed}" icon="angle-right"></font-awesome-icon>
          </a>
        </div>
        <div v-if="!this.editing" class='title'>
          {{title}}
        </div>
        <div v-else>
          <b-form-input
            type='text'
            ref="titleInput"
            placeholder='Título'
            v-model="cardTitle"
            @keyup.enter.native="cancelEditing()"
            class='font title_input card_title'
          ></b-form-input>
        </div>
        <div>
          <b-dropdown variant="link" toggle-class="test" no-caret right>
            <template slot="button-content">
              <span ><font-awesome-icon icon="ellipsis-v"></font-awesome-icon></span>
            </template>
            <b-dropdown-item @click="editCard()">Edit</b-dropdown-item>
            <b-dropdown-item @click="deleteCard()">Delete</b-dropdown-item>
          </b-dropdown>
        </div>
      </div>
      <div v-if="!this.collapsed" class='card_content'>
        <div v-if="!this.editing">
          {{this.content}}
        </div>
        <div v-else>
          <b-form-textarea
            type='text'
            v-model="content"
            @keyup.enter.native="cancelEditing()"
            class='font title_input card_title'
          ></b-form-textarea>
        </div>
      </div>
    </div>
</template>
<script>
export default {
  data: () => ({
    collapsed: false,
  }),
  methods: {
    collapse () {
      this.collapsed = !this.collapsed
    },
    editCard () {
      this.cardEditing = true
      this.collapsed = false
    },
    deleteCard () {

    },
    cancelEditing () {
      this.cardEditing = false
    },
  },
  mounted () {
    if (this.cardEditing) this.$refs.titleInput.focus()
  },
  updated () {
    if (this.cardEditing) this.$refs.titleInput.focus()
  },
  computed: {
    cardEditing: {
      get: function () {
        return this.editing
      },
      set: function (value) {
        this.$emit('update:editing', value)
      }
    },
    cardTitle: {
      get: function () {
        return this.title
      },
      set: function (value) {
        this.$emit('update:title', value)
      }
    }
  },
  props: ['id', 'title', 'content', 'editing']
}
</script>
<style>
.header{
  display: grid;
  grid-template-columns: 10% 80% 10%;
  background: #e4e4e4;
  border-bottom-style: solid;
  border-bottom-width: 1px;
  border-bottom-color: #d8d8d8;;
  border-top-left-radius: 5px;
  border-top-right-radius: 5px;
}
.header,.card_content{
  padding: 10px 0px;
}
.header svg {
  transition: .1s transform ease-in-out;
}
.header .collapsed svg {
  transform: rotate(90deg);
}
.inner{
  background: #fff;
  border-radius: 5px;
}
.title{
  grid-column-start: 2;
}
.arrow, .ellipsis{
  cursor: pointer;
}
.collapsed_card{
  border-radius: 5px;
}
.rotate{
  transition: ease-in-out .3s;
  transform: rotate(90deg);
}
.dropdown-item:hover, .dropdown-item:focus {
  background-color: #e4e4e4;
}
.dropdown-menu {
  min-width: 5rem;
}
.test{
  padding: 0 10px;
  border:0;
  color: #2c3e50;
}
.test:hover{
  color: #476e94;
}
.card_title{
  padding: 0px 10px;
  height: 100%;
}
</style>
