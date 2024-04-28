package LabExer6Aa;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
import java.util.Random;
import java.util.Scanner;

public class LabExer6Aa {

    private static List<String> wordsList = new ArrayList<>();
    private static Random random = new Random();
    private static String secretWord;
    private static StringBuilder guessedWord;

    public static void main(String[] args) {
        loadWords("words.txt");
        selectSecretWord();
        initializeGuessedWord();
        playGame();
    }

    private static void loadWords(String fileName) {
        try (BufferedReader reader = new BufferedReader(new FileReader(fileName))) {
            String line;
            while ((line = reader.readLine()) != null) {
                wordsList.add(line.trim());
            }
        } catch (IOException e) {
            System.err.println("Error reading words file: " + e.getMessage());
            System.exit(1);
        }
    }

    private static void selectSecretWord() {
        if (wordsList.isEmpty()) {
            System.err.println("No words found in the file.");
            System.exit(1);
        }
        secretWord = wordsList.get(random.nextInt(wordsList.size()));
    }

    private static void initializeGuessedWord() {
        guessedWord = new StringBuilder(secretWord.length());
        for (int i = 0; i < secretWord.length(); i++) {
            guessedWord.append(secretWord.charAt(i) == ' ' ? " " : "?");
        }
    }

    private static void playGame() {
        try (Scanner scanner = new Scanner(System.in)) {
            int attempts = 0;
            
            System.out.println("Welcome to the Guessing Game!");
            System.out.println("Try to guess the word: " + guessedWord);
            
            while (!guessedWord.toString().equals(secretWord)) {
                System.out.print("Enter a letter: ");
                char guess = scanner.nextLine().toUpperCase().charAt(0);
                
                boolean found = false;
                for (int i = 0; i < secretWord.length(); i++) {
                    if (secretWord.charAt(i) == guess) {
                        guessedWord.setCharAt(i, guess);
                        found = true;
                    }
                }
                
                if (found) {
                    System.out.println("Correct guess! Current progress: " + guessedWord);
                } else {
                    System.out.println("Incorrect guess. Try again.");
                    attempts++;
                }
            }
            
            System.out.println("Congratulations! You've guessed the word '" + secretWord + "' in " + attempts + " attempts.");
        }
    }
}
