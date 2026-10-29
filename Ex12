import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/* ---------- Main ---------- */
public class QuizApp {
    public static void main(String[] args) {
        Quiz quiz = new Quiz("Java Basics Online Quiz");
        quiz.addQuestion(new Question(1, "Which keyword is used to create an object?",
                new String[]{"class", "new", "this", "static"}, 2, 2));
        quiz.addQuestion(new Question(2, "Which collection class stores key-value pairs?",
                new String[]{"ArrayList", "HashSet", "HashMap", "LinkedList"}, 3, 2));
        quiz.addQuestion(new Question(3, "Which concept binds data and methods together?",
                new String[]{"Encapsulation", "Inheritance", "Polymorphism", "Abstraction"}, 1, 2));
        quiz.addQuestion(new Question(4, "Which block always executes after try-catch?",
                new String[]{"try", "catch", "finally", "throw"}, 3, 2));
        quiz.addQuestion(new Question(5, "Which access modifier gives the most restricted access?",
                new String[]{"public", "protected", "default", "private"}, 4, 2));

        QuizManager manager = new QuizManager(quiz);
        manager.showQuestions();

        Participant p1 = new Participant("Aarav");
        Participant p2 = new Participant("Meera");
        Participant p3 = new Participant("Rohan");

        manager.attempt(p1, new int[]{2, 3, 1, 3, 4});   // all correct
        manager.attempt(p2, new int[]{2, 1, 1, 9, 4});   // one wrong, one invalid option
        manager.attempt(p3, new int[]{1, 2, 2, 1, 3});   // all wrong

        manager.showResult(p1);
        manager.showResult(p2);
        manager.showResult(p3);
        manager.showLeaderboard();
    }
}

/* ---------- Custom Exception ---------- */
class InvalidAnswerException extends Exception {
    public InvalidAnswerException(String message) {
        super(message);
    }
}

/* ---------- Class & Object + Encapsulation: Question ---------- */
class Question {
    private int id;
    private String text;
    private String[] options;
    private int correctOption;   // 1-based index
    private int marks;

    public Question(int id, String text, String[] options, int correctOption, int marks) {
        this.id = id;
        this.text = text;
        this.options = options;
        this.correctOption = correctOption;
        this.marks = marks;
    }

    public int getId()          { return id; }
    public String getText()     { return text; }
    public int getMarks()       { return marks; }
    public String[] getOptions(){ return options.clone(); }

    public boolean isCorrect(int answer) {
        return answer == correctOption;
    }

    public String getCorrectAnswerText() {
        return options[correctOption - 1];
    }

    public void display() {
        System.out.println("Q" + id + ". " + text + "  [" + marks + " mark(s)]");
        for (int i = 0; i < options.length; i++) {
            System.out.println("    " + (i + 1) + ") " + options[i]);
        }
    }
}

/* ---------- Class & Object + Encapsulation: Participant ---------- */
class Participant {
    private String name;
    private int score;
    private int correctCount;
    private Map<Integer, Integer> answers = new HashMap<>();  // questionId -> chosen option

    public Participant(String name) {
        this.name = name;
    }

    public String getName()       { return name; }
    public int getScore()         { return score; }
    public int getCorrectCount()  { return correctCount; }
    public Map<Integer, Integer> getAnswers() { return answers; }

    public void recordAnswer(int questionId, int option) {
        answers.put(questionId, option);
    }

    public void addScore(int marks) {
        score += marks;
        correctCount++;
    }
}

/* ---------- Quiz: manages the question collection ---------- */
class Quiz {
    private String title;
    private List<Question> questions = new ArrayList<>();

    public Quiz(String title) {
        this.title = title;
    }

    public String getTitle() { return title; }

    public void addQuestion(Question q) {
        questions.add(q);
    }

    public List<Question> getQuestions() { return questions; }

    public int getTotalMarks() {
        int total = 0;
        for (Question q : questions) {
            total += q.getMarks();
        }
        return total;
    }

    /** Validates the answer, records it and updates the score. */
    public void submitAnswer(Participant p, Question q, int answer)
            throws InvalidAnswerException {
        if (answer < 1 || answer > q.getOptions().length) {
            throw new InvalidAnswerException(
                "Option " + answer + " is not valid for Q" + q.getId()
                + " (choose 1-" + q.getOptions().length + ")");
        }
        p.recordAnswer(q.getId(), answer);
        if (q.isCorrect(answer)) {
            p.addScore(q.getMarks());
        }
    }
}

/* ---------- QuizManager: runs the quiz and shows results ---------- */
class QuizManager {
    private Quiz quiz;
    private List<Participant> participants = new ArrayList<>();

    public QuizManager(Quiz quiz) {
        this.quiz = quiz;
    }

    public void showQuestions() {
        System.out.println("========== " + quiz.getTitle() + " ==========");
        System.out.println("Total Questions: " + quiz.getQuestions().size()
                + " | Total Marks: " + quiz.getTotalMarks() + "\n");
        for (Question q : quiz.getQuestions()) {
            q.display();
            System.out.println();
        }
    }

    public void attempt(Participant p, int[] chosen) {
        System.out.println("---- " + p.getName() + " is attempting the quiz ----");
        List<Question> qs = quiz.getQuestions();
        for (int i = 0; i < qs.size(); i++) {
            Question q = qs.get(i);
            try {
                quiz.submitAnswer(p, q, chosen[i]);
                System.out.println("Q" + q.getId() + " answered: option " + chosen[i]);
            } catch (InvalidAnswerException e) {
                System.out.println("Q" + q.getId() + " ERROR -> " + e.getMessage()
                        + " (skipped, 0 marks)");
            }
        }
        participants.add(p);
        System.out.println();
    }

    public void showResult(Participant p) {
        int total = quiz.getTotalMarks();
        double pct = (p.getScore() * 100.0) / total;
        System.out.println("*** RESULT: " + p.getName() + " ***");
        for (Question q : quiz.getQuestions()) {
            Integer given = p.getAnswers().get(q.getId());
            String status;
            if (given == null)           status = "NOT ANSWERED";
            else if (q.isCorrect(given)) status = "Correct";
            else                         status = "Wrong (Correct: " + q.getCorrectAnswerText() + ")";
            System.out.println("  Q" + q.getId() + ": " + status);
        }
        System.out.println("  Correct Answers : " + p.getCorrectCount() + "/" + quiz.getQuestions().size());
        System.out.println("  Score           : " + p.getScore() + "/" + total);
        System.out.printf("  Percentage      : %.1f%%%n", pct);
        System.out.println("  Final Result    : " + (pct >= 40 ? "PASS" : "FAIL"));
        System.out.println();
    }

    public void showLeaderboard() {
        System.out.println("========== LEADERBOARD ==========");
        List<Participant> sorted = new ArrayList<>(participants);
        sorted.sort((a, b) -> b.getScore() - a.getScore());
        int rank = 1;
        for (Participant p : sorted) {
            System.out.println(rank++ + ". " + p.getName() + " - " + p.getScore() + " marks");
        }
    }
}
