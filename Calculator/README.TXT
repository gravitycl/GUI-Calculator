import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class Calculator {
    // Declare variables
    private JFrame frame;
    private JTextField textField;
    private double firstNumber, secondNumber, result;
    private String operator;

    public Calculator() {
        // Create main frame
        frame = new JFrame("Calculator");
        frame.setSize(300, 400);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLayout(new BorderLayout());

        // Text field
        textField = new JTextField();
        textField.setFont(new Font("Arial", Font.BOLD, 20));
        textField.setHorizontalAlignment(SwingConstants.RIGHT);
        textField.setEditable(false);
        frame.add(textField, BorderLayout.NORTH);

        // Create buttons
        JPanel buttonPanel = new JPanel();
        buttonPanel.setLayout(new GridLayout(4, 4, 5, 5));

        String[] buttonLabels = {
            "7", "8", "9", "/",
            "4", "5", "6", "*",
            "1", "2", "3", "-",
            "C", "0", "=", "+"
        };

        for (String label : buttonLabels) {
            JButton button = new JButton(label);
            button.setFont(new Font("Arial", Font.BOLD, 18));
            button.addActionListener(new ButtonClickListener());
            buttonPanel.add(button);
        }

        frame.add(buttonPanel, BorderLayout.CENTER);

        // Show frame
        frame.setVisible(true);
    }

    private class ButtonClickListener implements ActionListener {
        @Override
        public void actionPerformed(ActionEvent e) {
            String command = ((JButton) e.getSource()).getText();

            switch (command) {
                case "C": // Clear
                    textField.setText("");
                    firstNumber = secondNumber = result = 0;
                    operator = "";
                    break;
                case "+": case "-": case "*": case "/":
                    firstNumber = Double.parseDouble(textField.getText());
                    operator = command;
                    textField.setText("");
                    break;
                case "=": // Calculate result
                    secondNumber = Double.parseDouble(textField.getText());
                    switch (operator) {
                        case "+": result = firstNumber + secondNumber; break;
                        case "-": result = firstNumber - secondNumber; break;
                        case "*": result = firstNumber * secondNumber; break;
                        case "/": 
                            if (secondNumber != 0) {
                                result = firstNumber / secondNumber;
                            } else {
                                textField.setText("Error");
                                return;
                            }
                            break;
                    }
                    textField.setText(String.valueOf(result));
                    break;
                default: // Numbers
                    textField.setText(textField.getText() + command);
            }
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(Calculator::new);
    }
}