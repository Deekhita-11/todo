# todo
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.io.*;
import java.util.ArrayList;

public class ToDoList extends JFrame {

    private DefaultListModel<String> taskModel;
    private JList<String> taskList;
    private JTextField taskField;
    private JButton addButton, deleteButton, saveButton, loadButton;

    public ToDoList() {
        setTitle("To-Do List");
        setSize(400, 400);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setLocationRelativeTo(null); // Center the window

        taskModel = new DefaultListModel<>();
        taskList = new JList<>(taskModel);
        JScrollPane scrollPane = new JScrollPane(taskList);

        taskField = new JTextField(20);
        addButton = new JButton("Add");
        deleteButton = new JButton("Delete");
        saveButton = new JButton("Save");
        loadButton = new JButton("Load");

        JPanel topPanel = new JPanel();
        topPanel.add(taskField);
        topPanel.add(addButton);

        
        JPanel bottomPanel = new JPanel();
        bottomPanel.add(deleteButton);
        bottomPanel.add(saveButton);
        bottomPanel.add(loadButton);

        add(topPanel, BorderLayout.NORTH);
        add(scrollPane, BorderLayout.CENTER);
        add(bottomPanel, BorderLayout.SOUTH);

       
        addButton.addActionListener(e -> {
            String task = taskField.getText().trim();
            if (!task.isEmpty()) {
                taskModel.addElement(task);
                taskField.setText("");
            }
        });

        deleteButton.addActionListener(e -> {
            int selected = taskList.getSelectedIndex();
            if (selected != -1) {
                taskModel.remove(selected);
            }
        });

        saveButton.addActionListener(e -> saveTasksToFile("tasks.txt"));
        loadButton.addActionListener(e -> loadTasksFromFile("tasks.txt"));

        setVisible(true);
    }

    private void saveTasksToFile(String filename) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(filename))) {
            for (int i = 0; i < taskModel.size(); i++) {
                writer.write(taskModel.get(i));
                writer.newLine();
            }
            JOptionPane.showMessageDialog(this, "Tasks saved!");
        } catch (IOException e) {
            JOptionPane.showMessageDialog(this, "Error saving tasks.");
        }
    }

    private void loadTasksFromFile(String filename) {
        try (BufferedReader reader = new BufferedReader(new FileReader(filename))) {
            taskModel.clear();
            String line;
            while ((line = reader.readLine()) != null) {
                taskModel.addElement(line);
            }
            JOptionPane.showMessageDialog(this, "Tasks loaded!");
        } catch (IOException e) {
            JOptionPane.showMessageDialog(this, "Error loading tasks.");
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(ToDoList::new);
    }
}
