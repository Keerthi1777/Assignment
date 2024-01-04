import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class PersistentURLShortener {
    private Map<String, String> shortToLongMap;
    private Map<String, String> longToShortMap;

    public PersistentURLShortener() {
        this.shortToLongMap = new HashMap<>();
        this.longToShortMap = new HashMap<>();
    }

    // Basic hash function for generating short URLs
    private String generateShortUrl(String longUrl) {
        // Example hash function: take the last 6 characters of the long URL
        int length = longUrl.length();
        return longUrl.substring(Math.max(0, length - 6), length);
    }

    // Function to shorten a long URL
    public String shortenUrl(String longUrl) {
        if (longToShortMap.containsKey(longUrl)) {
            return longToShortMap.get(longUrl);
        }

        String shortUrl = generateShortUrl(longUrl);

        if (shortToLongMap.containsKey(shortUrl)) {
            // Handle hash collision by appending a random number
            shortUrl += "-" + (int) (Math.random() * 1000);
        }

        shortToLongMap.put(shortUrl, longUrl);
        longToShortMap.put(longUrl, shortUrl);

        return shortUrl;
    }

    // Function to expand a short URL to its original form
    public String expandUrl(String shortUrl) {
        String longUrl = shortToLongMap.get(shortUrl);
        return longUrl != null ? longUrl : "URL not found";
    }

    // Save the link mappings to a persistent storage (e.g., a database)
    public void saveMappings() {
        // Implementation for saving to a persistent storage goes here
        System.out.println("Link mappings saved successfully.");
    }

    // Load the link mappings from a persistent storage (e.g., a database)
    public void loadMappings() {
        // Implementation for loading from a persistent storage goes here
        System.out.println("Link mappings loaded successfully.");
    }

    public static void main(String[] args) {
        PersistentURLShortener urlShortener = new PersistentURLShortener();

        // Load existing link mappings
        urlShortener.loadMappings();

        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("\nChoose an option:");
            System.out.println("1. Shorten URL");
            System.out.println("2. Expand URL");
            System.out.println("3. Save and Quit");

            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            switch (choice) {
                case 1:
                    System.out.println("Enter the long URL to shorten:");
                    String longUrl = scanner.nextLine();
                    String shortUrl = urlShortener.shortenUrl(longUrl);
                    System.out.println("Short URL: " + shortUrl);
                    break;

                case 2:
                    System.out.println("Enter the short URL to expand:");
                    String inputShortUrl = scanner.nextLine();
                    String expandedUrl = urlShortener.expandUrl(inputShortUrl);
                    System.out.println("Expanded URL: " + expandedUrl);
                    break;

                case 3:
                    // Save mappings and exit
                    urlShortener.saveMappings();
                    System.out.println("Exiting. Link mappings saved.");
                    System.exit(0);
                    break;

                default:
                    System.out.println("Invalid choice. Try again.");
            }
        }
    }
}
