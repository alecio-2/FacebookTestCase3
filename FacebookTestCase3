import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.openqa.selenium.By;
import org.openqa.selenium.Keys;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import java.io.File;
import java.io.IOException;
import java.util.List;

public class FacebookTestCase3 {
    private static final Logger LOGGER = LoggerFactory.getLogger(FacebookTestCase1.class);

    public static void main(String[] args) throws InterruptedException {
        LOGGER.info("Starting program");
        WebDriver driver = null;
        try {
            // Set the path to the chromedriver executable
            LOGGER.debug("Setting system property for chromedriver");
            System.setProperty("webdriver.chrome.driver", "D:\\Downloads\\chromedriver_win32 (1)\\chromedriver.exe");

            // Create ChromeOptions instance and disable notifications
            LOGGER.debug("Creating ChromeOptions instance");
            ChromeOptions options = new ChromeOptions();
            options.addArguments("--disable-notifications");

            // Create a new instance of the ChromeDriver with options
            LOGGER.debug("Creating ChromeDriver instance");
            driver = new ChromeDriver(options);

            // Navigate to the Facebook website
            LOGGER.info("Navigating to Facebook website");
            driver.get("https://www.facebook.com/");

            // Find the cookie notification and click the "Accept" or "Okay" button
            List<WebElement> cookieNotifications = driver.findElements(By.xpath("//button[@title='Tillåt endast nödvändiga cookies']"));
            if (cookieNotifications.size() > 0) {
                cookieNotifications.get(0).click();
                LOGGER.debug("Clicked on the cookie notification button");
            } else {
                LOGGER.info("No cookie notification found on the page");
            }

            // Json file with username and password to login
            LOGGER.debug("Loading JSON file with credentials");
            File jsonFile = new File("C:\\temp\\facebook.json");

            String email = null;
            String password = null;

            try {
                ObjectMapper objectMapper = new ObjectMapper();
                JsonNode jsonNode = objectMapper.readTree(jsonFile);

                email = jsonNode.get("facebookCredentials").get("email").asText();
                password = jsonNode.get("facebookCredentials").get("password").asText();

            } catch (IOException e) {
                e.printStackTrace();
            }


            // Enter login credentials and click login button
            LOGGER.info("Entering login credentials and clicking login button");
            WebElement emailInput = driver.findElement(By.id("email"));
            WebElement passwordInput = driver.findElement(By.id("pass"));
            emailInput.sendKeys(email);
            passwordInput.sendKeys(password);
            LOGGER.info("Credentials loaded");

            Thread.sleep(1000);

            WebElement loginButton = driver.findElement(By.xpath("//button[@name='login']"));
            loginButton.click();
            LOGGER.debug("Pressed login button");
            Thread.sleep(1000);

            // Verify that user is logged in and redirected to their profile page (either new user account page, or regular user account page)
            String expectedUrl1 = "https://www.facebook.com/";
            String expectedUrl2 = "https://www.facebook.com/?sk=welcome";
            String actualUrl = driver.getCurrentUrl();
            if (actualUrl.equals(expectedUrl1) || actualUrl.equals(expectedUrl2)) {
                LOGGER.debug("Login successful");
            } else {
                LOGGER.error("Login failed");
            }
            Thread.sleep(1000);


            // Case 3 starts here

            // Enter search query and click search button with aria-label="Search Facebook"
            String keyword = "Alex";
            WebElement searchBox = driver.findElement(By.xpath("//input[@aria-label='Search Facebook']"));
            searchBox.sendKeys(keyword);
            LOGGER.debug("Text entered in searchbar");
            Thread.sleep(1000);
            searchBox.sendKeys(Keys.RETURN);
            LOGGER.debug("ENTER pressed");
            Thread.sleep(3000);


            //Testing if the search has results
            List<WebElement> elements = driver.findElements(By.xpath("//*[contains(text(),'" + keyword + "')]"));
            if (elements.size() > 0) {
                LOGGER.info("Found " + elements.size() + " elements with text containing '" + keyword + "'");
                LOGGER.debug("Search successful");
            } else {
                LOGGER.error("No elements found with text containing '" + keyword + "'");
                LOGGER.error("Search failed");
            }
            Thread.sleep(2000);
            // Case 3 ends here

            //Logging out
            // Click on the profile button and wait for 2 seconds
            WebElement profileButton = driver.findElement(By.cssSelector("[aria-label='Your profile']"));
            profileButton.click();
            LOGGER.debug("Clicked on the profile button");
            Thread.sleep(1000);

            // Click logout button and verify that user is logged out and redirected to login page
            WebElement logoutButton = driver.findElement(By.xpath("//span[text()='Log Out']"));
            logoutButton.click();
            LOGGER.debug("Clicked on the logout button");
            Thread.sleep(1000);

            // Verify if user is logged out and redirected to login page
            String loginPageUrl = "https://www.facebook.com/";
            if (driver.getCurrentUrl().equals(loginPageUrl)) {
                LOGGER.debug("Logged out successfully");
            } else {
                LOGGER.error("Logout failed");
            }
            Thread.sleep(1000);

        } catch (InterruptedException e) {
            LOGGER.error("InterruptedException occurred: " + e.getMessage());
        } catch (Exception e) {
            LOGGER.error("Exception occurred: " + e.getMessage());
        } finally {
            // Close the browser
            driver.quit();
            LOGGER.info("Browser is closed");
            LOGGER.info("Program is finished");
            LOGGER.info(" ");
        }
    }
}
