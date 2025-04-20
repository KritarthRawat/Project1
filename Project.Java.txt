import java.io.*;
import java.util.Scanner;

public class FileOperations {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        String javaFile1Path = "JavaFile1.txt";
        System.out.println("Enter text for JavaFile1.txt (end with an empty line):");
        StringBuilder content1 = new StringBuilder();
        String line;
        while (!(line = scanner.nextLine()).isEmpty()) {
            content1.append(line).append(System.lineSeparator());
        }
        writeToFile(javaFile1Path, content1.toString());

        System.out.println("Content of JavaFile1.txt:");
        displayFileContent(javaFile1Path);

        String javaFile2Path = "JavaFile2.txt";
        System.out.println("Enter text for JavaFile2.txt:");
        String content2 = scanner.nextLine();
        writeToFile(javaFile2Path, content2 + System.lineSeparator());

        copyContent(javaFile1Path, javaFile2Path);

        analyzeFile(javaFile1Path, "polymorphism");

        scanner.close();
    }

    private static void writeToFile(String filePath, String content) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(filePath))) {
            writer.write(content);
        } catch (IOException e) {
            System.err.println("Error writing to file: " + e.getMessage());
        }
    }

    private static void displayFileContent(String filePath) {
        try (BufferedReader reader = new BufferedReader(new FileReader(filePath))) {
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            System.err.println("Error reading file: " + e.getMessage());
        }
    }

    private static void copyContent(String sourcePath, String destinationPath) {
        try (BufferedReader reader = new BufferedReader(new FileReader(sourcePath));
             BufferedWriter writer = new BufferedWriter(new FileWriter(destinationPath, true))) {
            String line;
            while ((line = reader.readLine()) != null) {
                writer.write(line);
                writer.newLine();
            }
        } catch (IOException e) {
            System.err.println("Error copying content: " + e.getMessage());
        }
    }

    private static void analyzeFile(String filePath, String wordToFind) {
        int totalCharacters = 0;
        int totalLines = 0;
        int totalWords = 0;
        int occurrences = 0;

        try (BufferedReader reader = new BufferedReader(new FileReader(filePath))) {
            String line;
            int lineNumber = 0;
            while ((line = reader.readLine()) != null) {
                lineNumber++;
                totalLines++;
                totalCharacters += line.length();
                totalWords += line.split("\\s+").length;

                if (line.contains(wordToFind)) {
                    occurrences++;
                    System.out.println("Found '" + wordToFind + "' in line: " + lineNumber);
                }
            }
        } catch (IOException e) {
            System.err.println("Error analyzing file: " + e.getMessage());
        }

        System.out.println("Total number of characters: " + totalCharacters);
        System.out.println("Total number of lines: " + totalLines);
        System.out.println("Total number of words: " + totalWords);
        System.out.println("Total occurrences of '" + wordToFind + "': " + occurrences);
    }
}
