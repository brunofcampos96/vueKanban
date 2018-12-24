<template>
  <div id='container'>
    <div id='boards' class='boards'>
      <Board
        v-bind="board"
        :editing.sync="board.editing"
        :title.sync="board.title"
        :supportBoard="false"
        v-for="board in boards" :key="board.id">
      </Board>
      <Board
        :supportBoard="true">
      </Board>
    </div>
  </div>
</template>
<script>
import Board from './board.vue'
export default {
  data: () => ({
    boards: [{
      id: 1,
      editing: false,
      title: 'Test1',
      cards: [{
        id: 1,
        title: 'Xablau',
        content: 'Teste',
        editing: false
      },{
        id: 2,
        title: 'CardTwhu',
        content: 'Testete',
        editing: false
      }],
      color: 'rgba(107, 0, 220, 0.3)',
      headColor: 'rgba(107, 0, 220, 0.5)'
    },
    {
      id: 2,
      editing: false,
      title: 'Test2',
      cards: [],
      color: 'rgba(0, 178, 220, 0.3)',
      headColor: 'rgba(0, 178, 220, 0.5)'
    }]
  }),
  methods: {
    addBoard () {
      let boards = this.boards
      let max = 0
      if (boards.length) {
        max = boards[0].id
        boards.forEach((board) => {
          if (board.id > max) max = board.id
        })
      }
      let colors = this.getRandomColor()
      let newBoard = {
        id: max + 1,
        editing: true,
        title: '',
        cards: [],
        color: colors[0],
        headColor: colors[1]
      }
      this.boards.push(newBoard)
    },
    deleteBoard (bId) {
      let boards = [...this.boards]
      let idxToRemove = boards.findIndex(board => board.id === bId)
      boards.splice(idxToRemove, 1)
      this.boards = boards
      if (!boards.length) this.addBoard()
    },
    getRandomColor () {
      let o = Math.round
      let r = Math.random
      let s = 255
      let c1 = o(r() * s)
      let c2 = o(r() * s)
      let c3 = o(r() * s)
      return ['rgba(' + c1 + ',' + c2 + ',' + c3 + ', 0.3)', 'rgba(' + c1 + ',' + c2 + ',' + c3 + ', 0.5)']
    }
  },
  components: {
    Board
  }
}
</script>
<style scoped>
#container{
  display: block;
  height: 100%;
}
.boards{
  display: grid;
  padding: 5px;
  grid-template-rows: 1fr;
  grid-auto-flow: column;
  grid-auto-columns: 300px;
  grid-column-gap: 5px;
  height: 100%;
}
</style>
