// Student Grade Tracer
import javax.swing.*;
import java.awt.*;
// import java.awt.event.*;
import java.util.*;

class Student {
    String name;
    java.util.List<Integer> grades;

    public Student(String name) {
        this.name = name;
        this.grades = new ArrayList<>();
    }

    public void addGrade(int grade) {
        grades.add(grade);
    }

    public double getAverage() {
        if (grades.isEmpty()) return 0;
        int sum = 0;
        for (int g : grades) sum += g;
        return (double) sum / grades.size();
    }

    public int getHighest() {
        return grades.isEmpty() ? 0 : Collections.max(grades);
    }

    public int getLowest() {
        return grades.isEmpty() ? 0 : Collections.min(grades);
    }

    public String toString() {
        return "Student: " + name + ", Grades: " + grades +
                ", Average: " + getAverage() +
                ", Highest: " + getHighest() +
                ", Lowest: " + getLowest();
    }
}

public class task1 extends JFrame {
    private java.util.List<Student> students = new ArrayList<>();
    private JTextArea displayArea;
    private JTextField nameField, gradeField;

    public task1() {
        setTitle("Student Grade Tracker");
        setSize(600, 400);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        JPanel inputPanel = new JPanel(new GridLayout(3, 2, 5, 5));
        inputPanel.setBorder(BorderFactory.createTitledBorder("Add Student/Grade"));

        inputPanel.add(new JLabel("Student Name:"));
        nameField = new JTextField();
        inputPanel.add(nameField);

        inputPanel.add(new JLabel("Grade:"));
        gradeField = new JTextField();
        inputPanel.add(gradeField);

        JButton addStudentBtn = new JButton("Add Student");
        JButton addGradeBtn = new JButton("Add Grade");
        inputPanel.add(addStudentBtn);
        inputPanel.add(addGradeBtn);

        add(inputPanel, BorderLayout.NORTH);

        displayArea = new JTextArea();
        displayArea.setEditable(false);
        add(new JScrollPane(displayArea), BorderLayout.CENTER);

        JButton viewBtn = new JButton("View All Students");
        add(viewBtn, BorderLayout.SOUTH);

        // Event Handlers
        addStudentBtn.addActionListener(e -> addStudent());
        addGradeBtn.addActionListener(e -> addGrade());
        viewBtn.addActionListener(e -> viewAllStudents());
    }

    private void addStudent() {
        String name = nameField.getText().trim();
        if (!name.isEmpty()) {
            students.add(new Student(name));
            displayArea.setText("Student added successfully!\n");
            nameField.setText("");
        } else {
            displayArea.setText("Enter a valid student name!\n");
        }
    }

    private void addGrade() {
        String name = nameField.getText().trim();
        String gradeText = gradeField.getText().trim();
        if (name.isEmpty() || gradeText.isEmpty()) {
            displayArea.setText("Enter student name and grade!\n");
            return;
        }

        try {
            int grade = Integer.parseInt(gradeText);
            for (Student s : students) {
                if (s.name.equalsIgnoreCase(name)) {
                    s.addGrade(grade);
                    displayArea.setText("Grade added successfully!\n");
                    gradeField.setText("");
                    return;
                }
            }
            displayArea.setText("Student not found!\n");
        } catch (NumberFormatException ex) {
            displayArea.setText("Invalid grade! Enter a number.\n");
        }
    }

    private void viewAllStudents() {
        if (students.isEmpty()) {
            displayArea.setText("No students available.\n");
            return;
        }

        StringBuilder report = new StringBuilder("--- Student Report ---\n");
        for (Student s : students) {
            report.append(s).append("\n");
        }
        displayArea.setText(report.toString());
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> new task1().setVisible(true));
    }
}
