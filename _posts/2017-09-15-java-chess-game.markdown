---
layout: post
title: "A simple chess game in Java"
categories: java
---
Calculating Knight's moves.

[Link to GitHub repo.](https://github.com/jamesharrop/learning-java/blob/master/ChessGame.java)

{% raw %}
```java
import java.util.Arrays;

public class ChessGame {
    public static void main(String[] args) {
        Board board = new Board();
        board.setKnightPos(0, 6);
        board.setKnightsMoveOptions();
        board.printBoard();    
    }
}

class Board {
    private char[][] boardArray = new char[8][8];
    private int x;
    private int y;
    
    public Board() {
        for(char[] row: boardArray)
            Arrays.fill(row, '-');
    }

    public void setKnightPos(int x, int y) {
        if (!isAValidSquare(x, y)) {
            System.out.println("Not a valid square");
        } else {
            this.x = x;
            this.y = y;
            boardArray[x][y] = 'K';
        }
    }

    public void printBoard() {
        for (int x = 0; x < 8; x++) {
            for (int y = 0; y < 8; y++)
                System.out.print(boardArray[x][y]);
            System.out.print("\n");
        }
    }

    public void setKnightsMoveOptions() {
        int[][] tryOffsets = {{-2,-1},{-2,+1},{-1,-2},{-1,+2},{+1,-2},
                                {+1,+2},{+2,-1},{+2,+1}};
        for (int n = 0; n < 8; n++) {
            int tryX = tryOffsets[n][0] + this.x;
            int tryY = tryOffsets[n][1] + this.y;
            if (isAValidSquare(tryX, tryY)) {
                boardArray[tryX][tryY] = '?';
            }
        }
    }

    private boolean isAValidSquare(int x, int y) {
        if ((x >= 0) && (x <= 7) && (y>=0) && (y <=7)) {
            return true;
        } else {
            return false;
        }
    }
}
```
{% endraw %}
