# Screenshot

import com.aventstack.extentreports.ExtentReports;
import com.aventstack.extentreports.reporter.ExtentHtmlReporter;

import java.io.FileInputStream;
import java.io.IOException;
import java.util.Properties;

public class ExtentManager {
    private static ExtentReports extent;

    public static ExtentReports getInstance() {
        if (extent == null) {
            String reportPath = getConfigProperty("base.folder") + "/extent-report.html";

            ExtentHtmlReporter htmlReporter = new ExtentHtmlReporter(reportPath);
            extent = new ExtentReports();
            extent.attachReporter(htmlReporter);
        }
        return extent;
    }

    private static String getConfigProperty(String key) {
        Properties properties = new Properties();
        try {
            FileInputStream fis = new FileInputStream("path/to/extent-config.properties");
            properties.load(fis);
            fis.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return properties.getProperty(key);
    }
}
