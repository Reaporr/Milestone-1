import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.image.BufferedImage;
import java.io.File;
import java.io.IOException;
import javax.imageio.ImageIO;

public class guitest {
    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> new GradeCalculatorGUI());
    }
}

class GradeCalculatorGUI {
    private JFrame frame;
    private JTextField milestone1Field;
    private JTextField milestone2Field;
    private JTextField terminalField;
    private JLabel resultLabel;
    private BufferedImage backgroundImage;

    public GradeCalculatorGUI() {
        
        try {
            backgroundImage = ImageIO.read(new File("D:\\motorph.jpg")); // Replace with your image path
        } catch (IOException e) {
            e.printStackTrace();
        }

        frame = new JFrame("Grade Calculator");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(400, 300);

        
        BackgroundPanel panel = new BackgroundPanel(backgroundImage);
        panel.setLayout(new GridBagLayout());

        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(10, 10, 10, 10);
        gbc.gridx = 0;
        gbc.gridy = 0;
        gbc.anchor = GridBagConstraints.WEST;

        JLabel milestone1Label = createStyledLabel("Milestone 1 (Max 25): ");
        milestone1Field = new JTextField(10);
        milestone1Field.setOpaque(false); 
        JLabel milestone2Label = createStyledLabel("Milestone 2 (Max 40): ");
        milestone2Field = new JTextField(10);
        milestone2Field.setOpaque(false); 
        JLabel terminalLabel = createStyledLabel("Terminal Assessment (Max 35): ");
        terminalField = new JTextField(10);
        terminalField.setOpaque(false); 
        JButton calculateButton = new JButton("Calculate Grade");
        resultLabel = createStyledLabel("");

        calculateButton.addActionListener(new CalculateButtonListener());

        panel.add(milestone1Label, gbc);
        gbc.gridx = 1;
        panel.add(milestone1Field, gbc);
        gbc.gridx = 0;
        gbc.gridy++;
        panel.add(milestone2Label, gbc);
        gbc.gridx = 1;
        panel.add(milestone2Field, gbc);
        gbc.gridx = 0;
        gbc.gridy++;
        panel.add(terminalLabel, gbc);
        gbc.gridx = 1;
        panel.add(terminalField, gbc);
        gbc.gridx = 0;
        gbc.gridy++;
        panel.add(calculateButton, gbc);
        gbc.gridx = 1;
        panel.add(resultLabel, gbc);

        frame.add(panel);
        frame.setVisible(true);
    }

    private JLabel createStyledLabel(String text) {
        JLabel label = new JLabel(text);
        label.setForeground(Color.WHITE); 
        return label;
    }

    private class CalculateButtonListener implements ActionListener {
        @Override
        public void actionPerformed(ActionEvent e) {
            try {
                int milestone1 = Integer.parseInt(milestone1Field.getText());
                int milestone2 = Integer.parseInt(milestone2Field.getText());
                int terminal = Integer.parseInt(terminalField.getText());

                GradeCalculator calculator = new GradeCalculator();
                if (calculator.validateScore(milestone1, 25) && 
                    calculator.validateScore(milestone2, 40) && 
                    calculator.validateScore(terminal, 35)) {
                    
                    double finalGrade = calculator.calculateGrade(milestone1, milestone2, terminal);
                    resultLabel.setText("Final Grade: " + finalGrade + "%");
                } else {
                    JOptionPane.showMessageDialog(frame, "Invalid input. Please enter valid scores.");
                }
            } catch (NumberFormatException ex) {
                JOptionPane.showMessageDialog(frame, "Please enter valid integer values.");
            }
        }
    }
}

class GradeCalculator {
    public boolean validateScore(int score, int max) {
        return score >= 0 && score <= max;
    }

    public double calculateGrade(int m1, int m2, int t) {
        return m1 + m2 + t;
    }
}


class BackgroundPanel extends JPanel {
    private BufferedImage image;

    public BackgroundPanel(BufferedImage image) {
        this.image = image;
    }

    @Override
    protected void paintComponent(Graphics g) {
        super.paintComponent(g);
        if (image != null) {
            g.drawImage(image, 0, 0, getWidth(), getHeight(), this);
        }
    }
}
