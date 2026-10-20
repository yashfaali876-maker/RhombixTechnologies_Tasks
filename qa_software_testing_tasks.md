# QA & Software Testing Internship Tasks - Rhombix Technologies

---

## Task 1: Basic Automation Using Selenium

### Objective
Write a basic automation script using Selenium WebDriver to open a webpage, perform a search query, and verify the resulting page title.

### Setup Instructions
1. Install Python on your system.
2. Install the required dependencies using pip:
   ```bash
   pip install selenium webdriver-manager
   ```

### Selenium Python Script (`task1_automation.py`)

```python
import time
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager

def run_test():
    # Set up Chrome WebDriver automatically
    service = Service(ChromeDriverManager().install())
    driver = webdriver.Chrome(service=service)
    
    try:
        # Step 1: Open Google
        driver.get("https://www.google.com")
        driver.maximize_window()
        print("Page opened successfully.")

        # Step 2: Locate search bar and submit query
        search_box = driver.find_element(By.NAME, "q")
        search_term = "Rhombix Technologies"
        search_box.send_keys(search_term + Keys.RETURN)
        
        time.sleep(3)

        # Step 3: Verify results
        assert search_term.lower() in driver.title.lower(), f"Verification failed! Title: {driver.title}"
        print(f"Test Passed: Page title contains '{search_term}'.")
        
    except Exception as e:
        print(f"Test Failed: {e}")
        
    finally:
        # Step 4: Close browser session
        driver.quit()

if __name__ == "__main__":
    run_test()
```

---

## Task 2: End-to-End Testing of a Web Application

### Objective
Perform end-to-end testing on a web application (SauceDemo E-Commerce site), including a Test Plan, Test Cases, Bug Report, Automated Script, and Test Summary Report.

---

### 1. Test Plan

* **Application Under Test:** SauceDemo (`https://www.saucedemo.com`)
* **Scope:** 
  * User Authentication (Login)
  * Product Browsing & Selection
  * Shopping Cart Operations
  * Checkout Workflow
* **Testing Methodology:** Combined Manual and Automated Testing (Selenium Python).

---

### 2. Test Cases Matrix

| Test Case ID | Module | Scenario | Execution Steps | Expected Result | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **TC01** | Login | Valid Login Credentials | 1. Navigate to URL<br>2. Enter valid credentials<br>3. Click Login | User is navigated to Products page | **PASS** |
| **TC02** | Login | Invalid Login Credentials | 1. Enter invalid credentials<br>2. Click Login | Error message is displayed | **PASS** |
| **TC03** | Cart | Add item to cart | 1. Click "Add to Cart" on an item<br>2. Observe cart badge | Cart badge updates count to `1` | **PASS** |
| **TC04** | Checkout | Complete Order Process | 1. Open cart<br>2. Click Checkout<br>3. Fill details and complete | Order confirmation header displayed | **PASS** |

---

### 3. Bug Report

* **Bug ID:** BUG-001
* **Title:** Cart item count badge fails to update immediately when an item is removed from within the Cart view.
* **Severity:** High
* **Priority:** High
* **Steps to Reproduce:**
  1. Add an item to the cart from the main inventory page.
  2. Click the cart icon to view cart items.
  3. Click "Remove" button next to the item.
* **Expected Result:** Item is removed and cart badge disappears.
* **Actual Result:** Item disappears from list, but cart icon still displays badge count `1` until page refresh.

---

### 4. Automated End-to-End Test Script (`task2_e2e_test.py`)

```python
import time
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager

def test_saucedemo_checkout():
    service = Service(ChromeDriverManager().install())
    driver = webdriver.Chrome(service=service)
    driver.implicitly_wait(10)
    
    try:
        # 1. Open Application
        driver.get("https://www.saucedemo.com/")
        
        # 2. Login
        driver.find_element(By.ID, "user-name").send_keys("standard_user")
        driver.find_element(By.ID, "password").send_keys("secret_sauce")
        driver.find_element(By.ID, "login-button").click()
        
        # 3. Add Item to Cart
        driver.find_element(By.ID, "add-to-cart-sauce-labs-backpack").click()
        
        # 4. Navigate to Cart
        driver.find_element(By.CLASS_NAME, "shopping_cart_link").click()
        
        # 5. Complete Checkout Process
        driver.find_element(By.ID, "checkout").click()
        driver.find_element(By.ID, "first-name").send_keys("Test")
        driver.find_element(By.ID, "last-name").send_keys("User")
        driver.find_element(By.ID, "postal-code").send_keys("54000")
        driver.find_element(By.ID, "continue").click()
        driver.find_element(By.ID, "finish").click()
        
        # 6. Verification
        header = driver.find_element(By.CLASS_NAME, "complete-header").text
        assert "THANK YOU FOR YOUR ORDER" in header.upper(), "Checkout verification failed"
        print("End-to-End Test Completed Successfully!")
        
    finally:
        driver.quit()

if __name__ == "__main__":
    test_saucedemo_checkout()
```

---

### 5. Summary Report

* **Total Executed:** 4 Test Cases
* **Passed:** 4
* **Bugs Identified:** 1
* **Overall Assessment:** Core workflow functions as expected with minor edge-case bug.

---

## Step-by-Step Submission Guide

1. **Upload to GitHub:** Create a repository named `RhombixTechnologies_Tasks` and upload your code files.
2. **Record Video:** Record a brief video demonstration explaining your code and execution.
3. **Post on LinkedIn:** Share your video on LinkedIn, include your GitHub repo link, and tag `@Rhombix Technologies`.
4. **Submit Form:** Fill out the official submission form with your GitHub and LinkedIn links.