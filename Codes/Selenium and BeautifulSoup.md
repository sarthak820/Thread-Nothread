import time
import urllib.request
import os
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from bs4 import BeautifulSoup  # Import BeautifulSoup

# Set up Selenium WebDriver (assuming you have the appropriate browser driver installed)
driver = webdriver.Chrome()

# Open the Google Images search page
driver.get("https://www.google.com/search?q=washers+images&tbm=isch&chips=q:washers,g_1:stainless+steel:tpTqz2Nuios%3D&rlz=1C1CHBD_enIN863IN863&hl=en&sa=X&ved=2ahUKEwj_-rSkivv_AhVJ7DgGHYf7Cz8Q4lYoBXoECAEQMg&biw=1519&bih=754")

# Scroll to the bottom of the page multiple times to load more images
scrolls = 6  # Change the number of scrolls as desired
for _ in range(scrolls):
    driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
    time.sleep(2)  # Wait for the page to load

# Parse the page source with BeautifulSoup
soup = BeautifulSoup(driver.page_source, "html.parser")

# Find the image URLs
image_urls = []
for img in soup.find_all('img'):
    image_url = img.get('src')
    if image_url:
        image_urls.append(image_url)

# Create a directory for storing the downloaded images
if not os.path.exists('washers'):
    os.makedirs('washers')

# Download the images
num_images_downloaded = 0
for i, url in enumerate(image_urls):
    if num_images_downloaded == 150:  # Change the limit to the desired number
        break
    try:
        filename = os.path.join('washers', 'washers{}.jpg'.format(i))
        urllib.request.urlretrieve(url, filename)
        print('Downloaded', url)
        num_images_downloaded += 1
    except Exception as e:
        print('Failed to download', url, e)

# Close the browser
driver.quit()
