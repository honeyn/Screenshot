# Screenshot

import org.openqa.selenium.OutputType;
import org.openqa.selenium.TakesScreenshot;
import org.openqa.selenium.WebDriver;

import com.aventstack.extentreports.ExtentReports;
import com.aventstack.extentreports.reporter.ExtentHtmlReporter;

import java.io.File;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.StandardCopyOption;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class ScreenshotUtils {

    public static void captureScreenshot(WebDriver driver, String folderPath, String screenshotName) {
        try {
            // Capture screenshot
            File screenshot = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);

            // Create the destination folder path with unique timestamp
            Path destinationFolder = createDestinationFolder(folderPath);

            // Generate unique file name with timestamp
            String fileName = generateFileName(screenshotName);

            // Set the file path
            Path filePath = destinationFolder.resolve("Screenshots").resolve(fileName);

            // Copy the screenshot file to the destination folder
            Files.copy(screenshot.toPath(), filePath.toAbsolutePath(), StandardCopyOption.REPLACE_EXISTING);

            System.out.println("Screenshot captured: " + filePath.toString());
        } catch (IOException e) {
            System.out.println("Failed to capture screenshot: " + e.getMessage());
        }
    }

    public static void saveExtentReport(String folderPath, ExtentReports extentReports) {
        try {
            // Create the destination folder path with unique timestamp
            Path destinationFolder = createDestinationFolder(folderPath);

            // Generate unique file name with timestamp
            String fileName = generateFileName("extent-html");

            // Set the file path for the HTML report
            Path filePath = destinationFolder.resolve(fileName);

            // Save the Extent HTML report
            ExtentHtmlReporter htmlReporter = new ExtentHtmlReporter(filePath.toFile());
            extentReports.attachReporter(htmlReporter);
            extentReports.setSystemInfo("Environment", "Production");
            extentReports.setSystemInfo("User", "John Doe");
            extentReports.flush();

            System.out.println("Extent HTML report saved: " + filePath.toString());
        } catch (IOException e) {
            System.out.println("Failed to save Extent HTML report: " + e.getMessage());
        }
    }

    private static Path createDestinationFolder(String folderPath) throws IOException {
        // Create the folder path with unique timestamp
        LocalDateTime now = LocalDateTime.now();
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd_HH-mm-ss");
        String timestamp = now.format(formatter);

        Path destinationFolder = new File(folderPath, "ABC_" + timestamp).toPath();
        if (!Files.exists(destinationFolder)) {
            Files.createDirectories(destinationFolder);
        }
        return destinationFolder;
    }
    private static String generateFileName(String name) {
        // Generate a unique file name based on current date and time
        LocalDateTime now = LocalDateTime.now();
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd_HH-mm-ss");
        String timestamp = now.format(formatter);
        String extension = ".png";
        return name + "_" + timestamp + extension;
    }
}

