int[][] board = new int[9][9]; 
int sizeCell = 56;
int r = -1, c = -1;           

void setup() {
  size(505, 505);
  loadGame("sudoku.txt"); 
  shuffleBoard();
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

void loadGame(String filename) {
  String[] lines = loadStrings(filename);
  int i = 0;
  while (i < 9) {
    String[] nums = split(lines[i], ' ');
    int j = 0;
    while (j < 9) {
      board[i][j] = int(nums[j]);
      j++;
    }
    i++;
  }
}

void drawGrid() {
  int i = 0;
  while (i <= 9) {
    strokeWeight((i % 3 == 0) ? 3 : 1);
    line(i * sizeCell, 0, i * sizeCell, height);
    line(0, i * sizeCell, width, i * sizeCell);
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
        text(board[i][j], j * sizeCell + sizeCell / 2, i * sizeCell + sizeCell / 2);
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

void shuffleBoard() {
  int total = 81;
  int[] flat = new int[total];
  int idx = 0;
  int i = 0;
  while (i < 9) {
    int j = 0;
    while (j < 9) {
      flat[idx] = board[i][j];
      idx++;
      j++;
    }
    i++;
  }

  int k = total - 1;
  while (k > 0) {
    int j = int(random(k + 1));
    int temp = flat[k];
    flat[k] = flat[j];
    flat[j] = temp;
    k--;
  }

  idx = 0;
  i = 0;
  while (i < 9) {
    int j = 0;
    while (j < 9) {
      board[i][j] = flat[idx];
      idx++;
      j++;
    }
    i++;
  }
}
