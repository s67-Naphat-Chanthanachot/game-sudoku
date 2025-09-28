int[][] board = new int[9][9]; 
int sizeCell = 60;
int r = -1, c = -1;           

void setup() {
  size(500, 500);
  loadGame("sudoku.txt"); 
}

void draw() {
  background(255);
  drawGrid();
  drawNumbers();
  highlightSelected();
}

void mousePressed() {
  r = mouseY / sizeCell;
  c = mouseX / sizeCell;
}

void loadGame(String fileName) {
  String[] lines = loadStrings(fileName);
  int i = 0;
  int row = 0;
  while (i < lines.length && row < 9) {
    String line = trim(lines[i]);
    if (line.length() > 0) {
      String[] nums = splitTokens(line, " \t");
      int j = 0;
      while (j < 9 && j < nums.length) {
        board[row][j] = int(nums[j]);
        j++;
      }
      row++;
    }
    i++;
  }
}

void drawGrid() {
  int i = 0;
  while (i <= 9) {
    strokeWeight((i % 3 == 0) ? 3 : 1);
    line(i*sizeCell, 0, i*sizeCell, height);
    line(0, i*sizeCell, width, i*sizeCell);
    i++;
  }
}

void drawNumbers() {
  textAlign(CENTER, CENTER);
  textSize(24);
  int i = 0;
  while (i < 9) {
    int j = 0;
    while (j < 9) {
      if (board[i][j] != 0) {
        fill(0);
        text(board[i][j], j*sizeCell + sizeCell/2, i*sizeCell + sizeCell/2);
      }
      j++;
    }
    i++;
  }
}

void highlightSelected() {
  if (r != -1 && c != -1) {
    noFill();
    stroke(255, 0, 0);
    strokeWeight(3);
    rect(c*sizeCell, r*sizeCell, sizeCell, sizeCell);
  }
}
