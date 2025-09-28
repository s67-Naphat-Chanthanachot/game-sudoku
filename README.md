int[][] board = new int[9][9]; 
int sizeCell = 60;

void setup() {
  size(500, 500);
}

void draw() {
  background(255);
  drawGrid();
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
