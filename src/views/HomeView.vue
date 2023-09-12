<script>

export default {
  
  data() {
    return {
      myScreen: [],
      screenRow: 30,
      LIShape: [[
                  ["🔲","🔳","🔳"],
                  ["🔲","🔲","🔲"]
                ],
                [
                  ["🔳","🔲"],
                  ["🔳","🔲"],
                  ["🔲","🔲"]
                ],
                [
                  ["🔲","🔲","🔲"],
                  ["🔳","🔳","🔲"]
                ],
                [
                  ["🔲","🔲"],
                  ["🔲","🔳"],
                  ["🔲","🔳"]
                ]],
      LShape: [[
                  ["🔳","🔳","🔲"],
                  ["🔲","🔲","🔲"]
                ],
                [
                  ["🔲","🔳"],
                  ["🔲","🔳"],
                  ["🔲","🔲"]
                ],
                [
                  ["🔲","🔲","🔲"],
                  ["🔲","🔳","🔳"]
                ],
                [
                  ["🔲","🔲"],
                  ["🔳","🔲"],
                  ["🔳","🔲"]
                ]],
      
      sqrShape: [[
                  ["🔲","🔲"],
                  ["🔲","🔲",]
                ],],
      currentIndex: 0,
      piece: [],
      pieceRow: 0,
      pieceCol: 0,
      updating: false,
    }
  },
  created() {
    for (let row = 0; row < this.screenRow; row++) {
      const myBox = [];
      for (let col = 0; col < 10; col++) {
        myBox[col] = "🔳";
      }
      this.myScreen[row] = myBox;
    }
    this.randomPiece()
    this.updatePiece();
  },
  mounted() {
    document.addEventListener('keydown', (e) => {
      if (e.key === 'ArrowRight') {
        this.movePieceRight()
      } else if (e.key === 'ArrowLeft') {
        this.movePieceLeft()
      } else if (e.key === ' ') { // Check for the spacebar key
        this.rotatePieceClockwise();

      } else if (e.key === 'ArrowDown') {
        this.dropPiece();
      }
      
    });
      // Set up a timer to move the piece down every second (1000 milliseconds)
    setInterval(this.movePieceDown, 1000);
  },
  
  methods: {
    randomPiece() {
      let min = 0;
      let max = 2;
      let randomNumber1 = Math.round(Math.random() * (max - min) + min);
      console.log("randomNumber1", randomNumber1)
      switch (randomNumber1) {
        case 0:
          this.piece = this.LIShape[this.currentIndex];
          return;
        case 1:
          this.piece = this.LShape[this.currentIndex];
          return;
        case 2:
          this.piece = this.sqrShape[this.currentIndex];
          return;
        default:
          this.piece = this.LIShape[this.currentIndex];
          return;
      }
    },
    updatePiece() {
      this.updating = true;
      // Clear the previous position of the piece on the screen
      for (let row = 0; row < this.screenRow; row++) {
        for (let col = 0; col < 10; col++) {
          if (this.myScreen[row][col] === "🔲") {
            this.myScreen[row][col] = "🔳";
          }
        }
      }
      // Place the piece on the screen based on pieceRow and pieceCol
      for (let row = 0; row < this.piece.length; row++) {
        for (let col = 0; col < this.piece[row].length; col++) {
          if (this.piece[row][col] === "🔲") {
            this.myScreen[this.pieceRow + row][this.pieceCol + col] = "🔲";
          }
        }
      }
      this.updating = false;
    },
    movePieceRight() {
      // Check if moving the piece to the right is possible
      if (this.pieceCol < 10 - this.piece[0].length) {
        // Clear the current position of the piece
        for (let row = 0; row < this.piece.length; row++) {
          for (let col = 0; col < this.piece[row].length; col++) {
            if (this.piece[row][col] === "🔲") {
              this.myScreen[this.pieceRow + row][this.pieceCol + col] = "🔳";
            }
          }
        }
        // Update the column position of the piece to move it right
        this.pieceCol++;

        // Place the piece in the new position
        this.updatePiece();
      // console.log("colF ", this.pieceCol )
      }
    },
    dropPiece() {
      // Keep moving the piece down until it can't move down anymore
      while (this.pieceRow + this.piece.length < this.screenRow) {
        this.movePieceDown();
      }
    },
    movePieceLeft() {
      // Check if moving the piece to the left is possible
      if (this.pieceCol > 0) {
        // Clear the current position of the piece
        for (let row = 0; row < this.piece.length; row++) {
          for (let col = 0; col < this.piece[row].length; col++) {
            if (this.piece[row][col] === "🔲") {
              this.myScreen[this.pieceRow + row][this.pieceCol + col] = "🔳";
            }
          }
        }

        // Update the column position of the piece to move it left
        this.pieceCol--;

        // Place the piece in the new position
        this.updatePiece();
        // console.log("colF ", this.pieceCol )

      }
    },
    movePieceDown() {
      // Check if moving the piece down is possible
      if (this.pieceRow + this.piece.length < this.screenRow) {
        // Clear the current position of the piece
        for (let row = 0; row < this.piece.length; row++) {
          for (let col = 0; col < this.piece[row].length; col++) {
            if (this.piece[row][col] === "🔲") {
              this.myScreen[this.pieceRow + row][this.pieceCol + col] = "🔳";
            }
          }
        }

        // Update the row position of the piece to move it down
        this.pieceRow++;

        // Place the piece in the new position
        this.counter = this.counter + 1;
        this.updatePiece();
      } else {
        // If moving down is not possible, generate a new piece at the top of the screen
        this.currentIndex = Math.floor(Math.random() * this.piece.length);
        this.piece = this.LIShape[this.currentIndex];
        this.pieceRow = 0;
        this.pieceCol = 0;
        this.updatePiece();
      }
    },

    rotatePieceClockwise() {
      if (this.updating === false) {
        // find piece shape with the index
        // console.log("this.piece", this.piece)
        this.currentIndex = this.LIShape.findIndex(shape => shape === this.piece);
        // console.log(this.currentIndex)
        // Calcula el siguiente índice para rotar la pieza a la derecha
        console.log("this.piece.length", this.piece, this.piece.length)
        let nextIndex = (this.currentIndex < 3) ? (this.currentIndex + 1) : 0;

        this.piece = this.LIShape[nextIndex]
      }
      
    }
  },
};


</script>
<template>
  <main class="main">
    <section>Usa las flechas para moverte y la barra espaciadora para rotar</section>
    <div v-for="(row, rowIndex) in myScreen" :key="rowIndex" className="screen__row">
      <div v-for="(cell, colIndex) in row" :key="colIndex" v-html="cell" className="screen__cell">
      </div>
    </div>
  </main>
</template>


<style scoped>
main {
  min-width:10em;
  max-width: 30em;
  background-color:  rgb(74, 99, 99);
  display:inline;
}
.screen__row {
  display: flex;
  flex-wrap: nowrap;
  justify-content: center;
  align-items: center;
}

.screen__cell {
  background:  rgb(0, 102, 128);
}
</style>