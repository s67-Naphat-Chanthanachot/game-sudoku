int[][] board = new int[9][9]; 
boolean[][] fixed = new boolean[9][9]; 
int sizeCell = 56;
int r = -1, c = -1;           

void setup() {
    size(505, 505);
    loadGame("sudoku.txt"); 
}

void draw() {
    background(255);
    drawGrid();
    drawNumbers();
    highlightSelected();
  
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
}

void keyPressed() { //ตอนกดคีย์บอร์ดใส่ตัวเลขลงในช่องที่เลือก
    if (r != -1 && c != -1 && !fixed[r][c]) {
      if (key == BACKSPACE || key == DELETE || key == '0') { //ถ้ากดbackspceหรือdeleteหรือ0คือจะลบเลขออก
        board[r][c] = 0;
      } else {
        int val = key - '0';
        if (val >= 1 && val <= 9) {
          if (board[r][c] == 0) {          
            if (checkValid(r, c, val)) {
              board[r][c] = val;
            } else {
              println("ใส่ไม่ได้ที่แถว " + r + " ช่อง " + c);
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
          if (fixed[i][j]) {
          fill(0); 
        } else {
          fill(50, 50, 200);
        }
        text(board[i][j], j*sizeCell + sizeCell/2, i*sizeCell + sizeCell/2);
      }
      j++;
    }
    i++;
  }
}

void highlightSelected() {
  if (r != -1 && c != -1) {
    noStroke();
    fill(150, 200, 255, 120);
    rect(c * sizeCell, r * sizeCell, sizeCell, sizeCell);
    noFill();
    stroke(0, 150, 255);
    strokeWeight(3);
    rect(c * sizeCell, r * sizeCell, sizeCell, sizeCell);
  }
}

