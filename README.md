int[][] board = new int[9][9]; 
boolean[][] fixed = new boolean[9][9]; 
int sizeCell;
int r = -1, c = -1;           
String message = "";

void setup() {
    fullScreen();
    sizeCell = min(width, height-200) / 9; 
    startGame();
}

void draw() {
    background(255);
    drawGrid();
    drawNumbers();
    highlightSelected();
    drawMessage();
    drawNumberPad();
}

void startGame() {
    loadGame("sudoku.txt");
    shuffleBoard();
    r = -1;
    c = -1;
    message = "";
}

void loadGame(String filename) {
    String[] lines = loadStrings(filename);
    int i = 0;
    while (i < 9) {
        String[] nums = split(lines[i], ' ');
        int j = 0;
        while (j < 9) {
            int val = int(nums[j]);
            board[i][j] = val;
            fixed[i][j] = (val != 0);
            j++;
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

void drawGrid() {
    int i = 0;
    while (i <= 9) {
        strokeWeight((i % 3 == 0) ? 4 : 1);
        line(i * sizeCell, 0, i * sizeCell, sizeCell*9);
        line(0, i * sizeCell, sizeCell*9, i * sizeCell);
        i++;
    }
}

void drawNumbers() {
    textAlign(CENTER, CENTER);
    textSize(sizeCell * 0.6);
    int i = 0;
    while (i < 9) {
        int j = 0;
        while (j < 9) {
            if (board[i][j] != 0) {
                if (fixed[i][j]) fill(0);
                else fill(50,50,200);
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
        fill(150, 200, 255, 100);
        rect(c*sizeCell, r*sizeCell, sizeCell, sizeCell);
        noFill();
        stroke(0,150,255);
        strokeWeight(4);
        rect(c*sizeCell, r*sizeCell, sizeCell, sizeCell);
    }
}

void drawMessage() {
    if (!message.equals("")) {
        fill(255,0,0);
        textSize(32);
        textAlign(CENTER, CENTER);
        text(message, width/2, sizeCell*9 + 50);
    }
}

void shuffleBoard() {
    int[] flat = new int[81];
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
    int k = 80;
    while (k > 0) {
        int j = int(random(k+1));
        int tmp = flat[k];
        flat[k] = flat[j];
        flat[j] = tmp;
        k--;
    }
    idx = 0;
    i = 0;
    while (i < 9) {
        int j = 0;
        while (j < 9) {
            board[i][j] = flat[idx];
            fixed[i][j] = (flat[idx] != 0);
            idx++;
            j++;
        }
        i++;
    }
}

void drawNumberPad() {
    int btnSize = sizeCell;
    int x0 = sizeCell*9 + 20;
    int y0 = 0;
    int n = 1;
    int row = 0;
    int col = 0;
    while (n <= 9) {
        int x = x0 + col * (btnSize + 5);
        int y = y0 + row * (btnSize + 5);
        fill(200);
        rect(x, y, btnSize, btnSize, 10);
        fill(0);
        textSize(24);
        textAlign(CENTER, CENTER);
        text(n, x + btnSize/2, y + btnSize/2);

        col++;
        if (col >= 3) { col = 0; row++; }
        n++;
    }
}

void mousePressed() {
    int mx = mouseX;
    int my = mouseY;
    int rowS = my / sizeCell;
    int colS = mx / sizeCell;
    if (rowS >= 0 && rowS < 9 && colS >= 0 && colS < 9) {
        r = rowS;
        c = colS;
    }

    int btnSize = sizeCell;
    int x0 = sizeCell*9 + 20;
    int y0 = 0;
    int n = 1;
    int row = 0;
    int col = 0;
    while (n <= 9) {
        int x = x0 + col * (btnSize + 5);
        int y = y0 + row * (btnSize + 5);
        if (mx > x && mx < x+btnSize && my > y && my < y+btnSize) {
            if (r != -1 && c != -1 && !fixed[r][c]) {
                if (checkValid(r,c,n)) {
                    board[r][c] = n;
                    message = "";
                } else {
                    message = "Cannot place " + n + " at (" + (r+1) + "," + (c+1) + ")";
                }
            }
        }
        col++;
        if (col >= 3) { col = 0; row++; }
        n++;
    }
}
