import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.util.function.Consumer;

public class BankBalanceApplication {

    private static double balance = 0.0;

    public static void main(String[] args) {
        SwingUtilities.invokeLater(BankBalanceApplication::createAndShowGUI);
    }

    private static void createAndShowGUI() {
        JFrame frame = new JFrame("BankBalanceApplication");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        JPanel mainPanel = new JPanel();
        mainPanel.setLayout(new FlowLayout());

        JLabel balanceLabel = new JLabel("Current Balance: $" + balance);
        JButton depositButton = createButton("Deposit", amount -> depositFunds(amount, balanceLabel));
        JButton withdrawButton = createButton("Withdraw", amount -> withdrawFunds(amount, balanceLabel));

        mainPanel.add(balanceLabel);
        mainPanel.add(depositButton);
        mainPanel.add(withdrawButton);

        frame.getContentPane().add(mainPanel);
        frame.setSize(300, 150);
        frame.setVisible(true);
    }

    private static JButton createButton(String label, Consumer<Double> action) {
        JButton button = new JButton(label);
        button.addActionListener(e -> {
            String amountString = JOptionPane.showInputDialog("Enter amount:");
            try {
                double amount = Double.parseDouble(amountString);
                action.accept(amount);
            } catch (NumberFormatException ex) {
                showError("Invalid input. Please enter a valid number.");
            }
        });
        return button;
    }

    private static void depositFunds(double amount, JLabel balanceLabel) {
        balance += amount;
        updateBalanceLabel(balanceLabel);
    }

    private static void withdrawFunds(double amount, JLabel balanceLabel) {
        if (amount > balance) {
            showError("Insufficient funds. Cannot withdraw.");
        } else {
            balance -= amount;
            updateBalanceLabel(balanceLabel);
        }
    }

    private static void updateBalanceLabel(JLabel balanceLabel) {
        balanceLabel.setText("Current Balance: $" + balance);
    }

    private static void showError(String message) {
        JOptionPane.showMessageDialog(null, message);
    }
}
