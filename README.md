import javafx.application.Application;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.Alert;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.GridPane;
import javafx.scene.layout.HBox;
import javafx.stage.Stage;

public class TicTacToeFX extends Application {
    // 3x3 board to track game state
    private char[][] board = new char[3][3];

    // Tracks current player ('X' or 'O')
    private char currentPlayer = 'X';

    // 2D array of buttons representing the game grid
    private Button[][] buttons = new Button[3][3];

    // Score counters for each player
    private int scoreX = 0;
    private int scoreO = 0;

    // Label to display the score
    private Label scoreLabel;

    @Override
    public void start(Stage primaryStage) {
        initializeBoard(); // Fill board with empty spaces

        // Create a grid layout for the game board
        GridPane grid = new GridPane();
        grid.setAlignment(Pos.CENTER);

        // Create 3x3 buttons and add them to the grid
        for (int row = 0; row < 3; row++) {
            for (int col = 0; col < 3; col++) {
                Button button = new Button(" "); // Empty button
                button.setPrefSize(100, 100);    // Set size
                final int r = row;
                final int c = col;

                // Set what happens when the button is clicked
                button.setOnAction(e -> handleMove(r, c, button));

                buttons[row][col] = button;      // Store button in array
                grid.add(button, col, row);      // Add to grid layout
            }
        }

        // Create a reset button to restart the game
        Button resetButton = new Button("Reset");
        resetButton.setOnAction(e -> resetGame());

        // Create a label to show the score
        scoreLabel = new Label("Score - X: 0 | O: 0");

        // Layout for score and reset button
        HBox controls = new HBox(20, scoreLabel, resetButton);
        controls.setAlignment(Pos.CENTER);

        // Main layout: grid in center, controls at bottom
        BorderPane root = new BorderPane();
        root.setCenter(grid);
        root.setBottom(controls);

        // Set up the scene and show the window
        Scene scene = new Scene(root, 320, 370);
        primaryStage.setTitle("Tic Tac Toe");
        primaryStage.setScene(scene);
        primaryStage.show();
    }

    // Handles a player's move when a button is clicked
    private void handleMove(int row, int col, Button button) {
        if (board[row][col] != ' ') return; // Ignore if already filled

        board[row][col] = currentPlayer;    // Update board state
        button.setText(String.valueOf(currentPlayer)); // Show symbol on button

        if (checkWin(currentPlayer)) {
            showAlert("Player " + currentPlayer + " wins!"); // Show win message
            updateScore(currentPlayer);                      // Update score
            disableAllButtons();                             // Lock board
        } else if (isBoardFull()) {
            showAlert("It's a draw!"); // Show draw message
        } else {
            // Switch to the other player
            currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
        }
    }

    // Resets the game board for a new round
    private void resetGame() {
        currentPlayer = 'X'; // Start with X again
        initializeBoard();   // Clear board state

        // Reset all buttons to empty and enable them
        for (int i = 0; i < 3; i++)
            for (int j = 0; j < 3; j++) {
                buttons[i][j].setText(" ");
                buttons[i][j].setDisable(false);
            }
    }

    // Updates the score and refreshes the score label
    private void updateScore(char winner) {
        if (winner == 'X') scoreX++;
        else if (winner == 'O') scoreO++;
        scoreLabel.setText("Score - X: " + scoreX + " | O: " + scoreO);
    }

    // Initializes the board with empty spaces
    private void initializeBoard() {
        for (int i = 0; i < 3; i++)
            for (int j = 0; j < 3; j++)
                board[i][j] = ' ';
    }

    // Checks if the given player has won
    private boolean checkWin(char player) {
        // Check rows and columns
        for (int i = 0; i < 3; i++) {
            if (board[i][0] == player && board[i][1] == player && board[i][2] == player) return true;
            if (board[0][i] == player && board[1][i] == player && board[2][i] == player) return true;
        }
        // Check diagonals
        return (board[0][0] == player && board[1][1] == player && board[2][2] == player) ||
               (board[0][2] == player && board[1][1] == player && board[2][0] == player);
    }

    // Checks if the board is full (no empty spaces)
    private boolean isBoardFull() {
        for (char[] row : board)
            for (char cell : row)
                if (cell == ' ') return false;
        return true;
    }

    // Disables all buttons after game ends
    private void disableAllButtons() {
        for (Button[] row : buttons)
            for (Button b : row)
                b.setDisable(true);
    }

    // Shows a popup alert with the given message
    private void showAlert(String message) {
        Alert alert = new Alert(Alert.AlertType.INFORMATION);
        alert.setTitle("Game Over");
        alert.setHeaderText(null);
        alert.setContentText(message);
        alert.show();
    }

    // Launches the JavaFX application
    public static void main(String[] args) {
        launch(args);
    }
}

