  1  import javafx.application.Application;
  2  import javafx.stage.Stage;
  3  import javafx.scene.Scene;
  4  import javafx.scene.control.Label;
  5  import javafx.scene.layout.BorderPane;
  6  import javafx.scene.layout.GridPane;
  7  import javafx.scene.layout.Pane;
  8  import javafx.scene.paint.Color;
  9  import javafx.scene.shape.Line;
 10  import javafx.scene.shape.Ellipse;
 11  
 12  public class TicTacToe extends Application {
 13    // Indicate which player has a turn, initially it is the X player
 14    private char whoseTurn = 'X';
 15  
 16    // Create and initialize cell
 17    private Cell[][] cell =  new Cell[3][3];
 18  
 19    // Create and initialize a status label
 20    private Label lblStatus = new Label("X's turn to play");
 21  
 22    @Override // Override the start method in the Application class
 23    public void start(Stage primaryStage) {
 24      // Pane to hold cell
 25      GridPane pane = new GridPane(); 
 26      for (int i = 0; i < 3; i++)
 27        for (int j = 0; j < 3; j++)
 28          pane.add(cell[i][j] = new Cell(), j, i);
 29  
 30      BorderPane borderPane = new BorderPane();
 31      borderPane.setCenter(pane);
 32      borderPane.setBottom(lblStatus);
 33      
 34      // Create a scene and place it in the stage
 35      Scene scene = new Scene(borderPane, 450, 170);
 36      primaryStage.setTitle("TicTacToe"); // Set the stage title
 37      primaryStage.setScene(scene); // Place the scene in the stage
 38      primaryStage.show(); // Display the stage   
 39    }
 40  
 41    /** Determine if the cell are all occupied */
 42    public boolean isFull() {
 43      for (int i = 0; i < 3; i++)
 44        for (int j = 0; j < 3; j++)
 45          if (cell[i][j].getToken() == ' ')
 46            return false;
 47  
 48      return true;
 49    }
 50  
 51    /** Determine if the player with the specified token wins */
 52    public boolean isWon(char token) {
 53      for (int i = 0; i < 3; i++)
 54        if (cell[i][0].getToken() == token
 55            && cell[i][1].getToken() == token
 56            && cell[i][2].getToken() == token) {
 57          return true;
 58        }
 59  
 60      for (int j = 0; j < 3; j++)
 61        if (cell[0][j].getToken() ==  token
 62            && cell[1][j].getToken() == token
 63            && cell[2][j].getToken() == token) {
 64          return true;
 65        }
 66  
 67      if (cell[0][0].getToken() == token 
 68          && cell[1][1].getToken() == token        
 69          && cell[2][2].getToken() == token) {
 70        return true;
 71      }
 72  
 73      if (cell[0][2].getToken() == token
 74          && cell[1][1].getToken() == token
 75          && cell[2][0].getToken() == token) {
 76        return true;
 77      }
 78  
 79      return false;
 80    }
 81  
 82    // An inner class for a cell
 83    public class Cell extends Pane {
 84      // Token used for this cell
 85      private char token = ' ';
 86  
 87      public Cell() {
 88        setStyle("-fx-border-color: black"); 
 89        this.setPrefSize(800, 800);
 90        this.setOnMouseClicked(e -> handleMouseClick());
 91      }
 92  
 93      /** Return token */
 94      public char getToken() {
 95        return token;
 96      }
 97  
 98      /** Set a new token */
 99      public void setToken(char c) {
100        token = c;
101        
102        if (token == 'X') {
103          Line line1 = new Line(10, 10, 
104            this.getWidth() - 10, this.getHeight() - 10);
105          line1.endXProperty().bind(this.widthProperty().subtract(10));
106          line1.endYProperty().bind(this.heightProperty().subtract(10));
107          Line line2 = new Line(10, this.getHeight() - 10, 
108            this.getWidth() - 10, 10);
109          line2.startYProperty().bind(
110            this.heightProperty().subtract(10));
111          line2.endXProperty().bind(this.widthProperty().subtract(10));
112          
113          // Add the lines to the pane
114          this.getChildren().addAll(line1, line2); 
115        }
116        else if (token == 'O') {
117          Ellipse ellipse = new Ellipse(this.getWidth() / 2, 
118            this.getHeight() / 2, this.getWidth() / 2 - 10, 
119            this.getHeight() / 2 - 10);
120          ellipse.centerXProperty().bind(
121            this.widthProperty().divide(2));
122          ellipse.centerYProperty().bind(
123              this.heightProperty().divide(2));
124          ellipse.radiusXProperty().bind(
125              this.widthProperty().divide(2).subtract(10));        
126          ellipse.radiusYProperty().bind(
127              this.heightProperty().divide(2).subtract(10));   
128          ellipse.setStroke(Color.BLACK);
129          ellipse.setFill(Color.WHITE);
130          
131          getChildren().add(ellipse); // Add the ellipse to the pane
132        }
133      }
134  
135      /* Handle a mouse click event */
136      private void handleMouseClick() {
137        // If cell is empty and game is not over
138        if (token == ' ' && whoseTurn != ' ') {
139          setToken(whoseTurn); // Set token in the cell
140  
141          // Check game status
142          if (isWon(whoseTurn)) {
143            lblStatus.setText(whoseTurn + " won! The game is over");
144            whoseTurn = ' '; // Game is over
145          }
146          else if (isFull()) {
147            lblStatus.setText("Draw! The game is over");
148            whoseTurn = ' '; // Game is over
149          }
150          else {
151            // Change the turn
152            whoseTurn = (whoseTurn == 'X') ? 'O' : 'X';
153            // Display whose turn
154            lblStatus.setText(whoseTurn + "'s turn");
155          }
156        }
157      }
158    }
159    
160    /**
161     * The main method is only needed for the IDE with limited
162     * JavaFX support. Not needed for running from the command line.
163     */
164    public static void main(String[] args) {
165      launch(args);
166    }
167  }
