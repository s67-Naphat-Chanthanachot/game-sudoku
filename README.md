int[][] board = new int[9][9]; 
boolean[][] fixed = new boolean[9][9]; 
int sizeCell = 56;
int r = -1, c = -1;           
String message = "";

void setup() {
  size(505, 560);
  loadGame("sudoku.txt"); 
}

void draw() {
  background(255);
  pushMatrix();
  drawGrid();
  drawNumbers();
  highlightSelected();
  popMatrix();

  stroke(0);
  strokeWeight(2);
  line(0, 505, width, 505);

  fill(0);
  textAlign(CENTER, CENTER);
  textSize(20);
  text(message, width / 2, 530);

  if (checkWin()) {
    fill(0, 150, 0);
    textSize(32);
    textAlign(CENTER, CENTER);
    text("You Win!", width/2, height/2);
    noLoop();
  }
}

void mousePressed() {
  r = mouseY / sizeCell;
  c = mouseX / sizeCell;
  if (r < 9 && c < 9) {
    message = "selected cell (" + (r+1) + "," + (c+1) + ")";
  }
}

void keyPressed() {
  if (r != -1 && c != -1 && r < 9 && c < 9 && !fixed[r][c]) {
    if (key == BACKSPACE || key == DELETE || key == '0') {
      board[r][c] = 0;
      message = "clear cell (" + (r+1) + "," + (c+1) + ")";
    } else {
      int val = key - '0';
      if (val >= 1 && val <= 9) {
        if (board[r][c] == 0) {
          if (checkValid(r, c, val)) {
            board[r][c] = val;
            message = "place " + val + " at (" + (r+1) + "," + (c+1) + ")";
          } else {
            message = "can't place " + val + " at (" + (r+1) + "," + (c+1) + ")";
          }
        }
      }
    }
  }
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
        int val = int(nums[j]);
        board[row][j] = val;
        if (val != 0) {
          fixed[row][j] = true; 
        }
        j++;
      }
      row++;
    }
    i++;
  }
}

boolean checkValid(int row, int col, int val) {
  int i = 0;
  while (i < 9) {
    if (board[row][i] == val) return false; 
    if (board[i][col] == val) return false; 
    i++;
  }
  int startR = (row / 3) * 3;
  int startC = (col / 3) * 3;
  i = 0;
  while (i < 3) {
    int j = 0;
    while (j < 3) {
      if (board[startR + i][startC + j] == val) return false; 
      j++;
    }
    i++;
  }
  return true;
}

boolean checkWin() {
  int i = 0;
  while (i < 9) {
    int j = 0;
    while (j < 9) {
      int val = board[i][j];
      if (val == 0) return false;
      board[i][j] = 0;
      if (!checkValid(i, j, val)) {
        board[i][j] = val;
        return false;
      }
      board[i][j] = val;
      j++;
    }
    i++;
  }
  return true;
}

void drawGrid() {
  int i = 0;
  while (i <= 9) {
    strokeWeight((i % 3 == 0) ? 3 : 1);
    line(i * sizeCell, 0, i * sizeCell, 9 * sizeCell);
    line(0, i * sizeCell, 9 * sizeCell, i * sizeCell);
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
        if (fixed[i][j]) fill(0); 
        else fill(50, 50, 200);
        text(board[i][j], j*sizeCell + sizeCell/2, i*sizeCell + sizeCell/2);
      }
      j++;
    }
    i++;
  }
}

void highlightSelected() {
  if (r != -1 && c != -1 && r < 9 && c < 9) {
    noStroke();
    fill(150, 200, 255, 120);
    rect(c * sizeCell, r * sizeCell, sizeCell, sizeCell);
    noFill();
    stroke(0, 150, 255);
    strokeWeight(3);
    rect(c * sizeCell, r * sizeCell, sizeCell, sizeCell);
  }
}
