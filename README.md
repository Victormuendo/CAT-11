# CAT-II
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.sql.*;

public class RegistrationForm extends JFrame {
    // GUI components
    private JLabel nameLabel, emailLabel;
    private JTextField nameField, emailField;
    private JButton registerButton;

    // Database connection
    private Connection conn;
    private PreparedStatement pstmt;

    // Constructor
    public RegistrationForm() {
        // Initialize JFrame
        setTitle("Registration Form");
        setSize(400, 200);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null); // Center the frame
        setLayout(new GridLayout(3, 2));

        // Initialize components
        nameLabel = new JLabel("Name:");
        nameField = new JTextField();
        emailLabel = new JLabel("Email:");
        emailField = new JTextField();
        registerButton = new JButton("Register");

        // Add components to the JFrame
        add(nameLabel);
        add(nameField);
        add(emailLabel);
        add(emailField);
        add(registerButton);

        // Event listener for the register button
        registerButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                registerUser();
            }
        });

        // Establish database connection
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/your_database", "username", "password");
            pstmt = conn.prepareStatement("INSERT INTO users (name, email) VALUES (?, ?)");
        } catch (Exception ex) {
            ex.printStackTrace();
        }

        // Display the JFrame
        setVisible(true);
    }

    // Method to register a user into the database
    private void registerUser() {
        String name = nameField.getText().trim();
        String email = emailField.getText().trim();

        try {
            pstmt.setString(1, name);
            pstmt.setString(2, email);
            pstmt.executeUpdate();
            JOptionPane.showMessageDialog(this, "User registered successfully!");
        } catch (SQLException ex) {
            ex.printStackTrace();
        }
    }

    // Main method to run the application
    public static void main(String[] args) {
        // Create the registration form
        new RegistrationForm();
    }
}
