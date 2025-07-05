import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.Random;

public class NumberGameGUI extends JFrame {
    private static final int RANGE_START = 1;
    private static final int RANGE_END = 100;
    private static final int MAX_ATTEMPTS = 7;

    private final Random random = new Random();
    private int numberToGuess;
    private int attempts;
    private int totalScore;

    private final JLabel promptLabel = new JLabel("Guess a number between 1 and 100:");
    private final JTextField guessField = new JTextField(10);
    private final JButton guessButton = new JButton("Guess");
    private final JLabel feedbackLabel = new JLabel(" ");
    private final JLabel attemptsLabel = new JLabel("Attempts left: " + MAX_ATTEMPTS);
    private final JLabel scoreLabel = new JLabel("Score: 0");
    private final JButton newRoundButton = new JButton("New Round");

    public NumberGameGUI() {
        super("Number Game");
        setLayout(new BorderLayout());

        JPanel topPanel = new JPanel();
        topPanel.add(promptLabel);
        topPanel.add(guessField);
        topPanel.add(guessButton);

        JPanel centerPanel = new JPanel(new GridLayout(3, 1));
        centerPanel.add(feedbackLabel);
        centerPanel.add(attemptsLabel);
        centerPanel.add(scoreLabel);

        JPanel bottomPanel = new JPanel();
        bottomPanel.add(newRoundButton);

        add(topPanel, BorderLayout.NORTH);
        add(centerPanel, BorderLayout.CENTER);
        add(bottomPanel, BorderLayout.SOUTH);

        guessButton.addActionListener(new GuessHandler());
        newRoundButton.addActionListener(e -> startNewRound());

        startNewRound();

        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setSize(400, 200);
        setVisible(true);
    }

    private class GuessHandler implements ActionListener {
        public void actionPerformed(ActionEvent e) {
            String input = guessField.getText();
            int guess;

            try {
                guess = Integer.parseInt(input);
            } catch (NumberFormatException ex) {
                feedbackLabel.setText("Enter a valid integer.");
                return;
            }

            attempts++;

            if (guess == numberToGuess) {
                feedbackLabel.setText("Correct!");
                totalScore += MAX_ATTEMPTS - attempts + 1;
                scoreLabel.setText("Score: " + totalScore);
                endRound();
            } else if (guess < numberToGuess) {
                feedbackLabel.setText("Too low.");
            } else {
                feedbackLabel.setText("Too high.");
            }

            attemptsLabel.setText("Attempts left: " + (MAX_ATTEMPTS - attempts));

            if (attempts >= MAX_ATTEMPTS && guess != numberToGuess) {
                feedbackLabel.setText("Out of attempts. The number was " + numberToGuess + ".");
                endRound();
            }
        }
    }

    private void startNewRound() {
        numberToGuess = random.nextInt(RANGE_END - RANGE_START + 1) + RANGE_START;
        attempts = 0;
        feedbackLabel.setText(" ");
        attemptsLabel.setText("Attempts left: " + MAX_ATTEMPTS);
        guessField.setText("");
        guessField.setEditable(true);
        guessButton.setEnabled(true);
    }

    private void endRound() {
        guessField.setEditable(false);
        guessButton.setEnabled(false);
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(NumberGameGUI::new);
    }
}
