<template>
  <div class="wrapper" :class="[enabled ? 'enabled' : '']">
    <div class="slot"
         :style="{ color: color }" v-for="piece in pieces" :key="piece.type"
         :class="[piece.count > 0 ? 'show' : 'hide']">
      <div class="piece" @mousedown="clicked($event, piece.type, color, piece.count)"
          :class="[piece.type, color, piece.type === selectedPiece ? 'selected' : '' ]"></div>
      <div class="piece-count noselect" :class="[piece.count > 1 ? 'show' : 'invisible']">{{piece.count}}</div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'ChessPocket',
  computed: {
    emptySlots: function () {
      return Math.max(8 - this.pieces.length, 0)
    },
    pieceStyle () {
      return this.$store.getters.pieceStyle
    }
  },
  watch: {
    pieceStyle: function (pieceStyle) {
      this.updatePieceCSS(pieceStyle)
    }
  },
  props: {
    color: {
      type: String,
      default: 'white'
    },
    pieces: {
      type: Array,
      default: () => ([
        {'count': 0, 'type': 'pawn'},
        {'count': 0, 'type': 'knight'},
        {'count': 0, 'type': 'bishop'},
        {'count': 0, 'type': 'rook'},
        {'count': 0, 'type': 'queen'}
      ])
    },
    enabled: {
      type: Boolean,
      default: true
    },
    selectedPiece: {
      type: String,
      default: ''
    }
  },
  methods: {
    clicked (event, pieceType, color, pieceCount) {
      if (pieceCount === 0) {
        return
      }
      console.log(`pieceType: ${pieceType}`)
      console.log(`color: ${color}`)
      this.$emit('selection', event, pieceType, color)
    },
    updatePieceCSS (pieceStyle) {
      let file = document.createElement('link')
      file.rel = 'stylesheet'
      file.href = 'assets/images/piece-css/' + pieceStyle + '.css'
      document.head.appendChild(file)
    }
  }
}
</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style scoped>
.wrapper {
  text-align: left;
}
.wrapper.enabled {
  cursor: pointer;
}
.piece-count {
  text-align: center;
  font-size: 12pt;
  margin-left:38px;
  margin-top:-25px;
  width: 18px;
  padding: 0px 0px 0px 0px;
  border-radius: 4px 4px 4px 4px;
  background-color: rgb(221, 221, 221);
  background-color: #cccfd7;
  border-color: #222;
  background-color: #fff;
  border-width: 1px;
  border-style: solid;
  color: black;
}
.slot {
  width: 60px;
  height: 60px;
  border-radius: 3px 3px 3px 3px;
  vertical-align: middle;
  font-size: 35px;
}
.slot:hover {
  background-color: #ddd;
  color: black;
}
.piece {
  height: 60px;
  width: 60px;
  background-size: 60px;
}
.piece.selected {
  background-color: rgb(112, 201, 146);
}
.invisible {
    opacity: 0;
}
.hide {
    opacity: 0.25;
}
.show {
    opacity: 1;
}
.show:hover {
  cursor: pointer;
}
.wrapper {
  background-size: 90% 90%;
  background-repeat: no-repeat;
}
</style>
