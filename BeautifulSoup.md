import requests
from bs4 import BeautifulSoup
import urllib.request
import os

url = "https://www.google.com/search?q=threaded+nuts&tbm=isch&ved=2ahUKEwi-yMzr7YuAAxV-kWMGHcJkCDUQ2-cCegQIABAA&oq=threaded+nuts&gs_lcp=CgNpbWcQAzIFCAAQgAQyBQgAEIAEMgYIABAHEB4yBggAEAcQHjIGCAAQBxAeMgYIABAHEB4yBggAEAcQHjIGCAAQBxAeMgYIABAHEB4yBggAEAcQHjoECCMQJzoKCAAQigUQsQMQQzoICAAQgAQQsQNQtR5YqS1gqjBoAHAAeACAAdEBiAH_DpIBBTAuNy4zmAEAoAEBqgELZ3dzLXdpei1pbWfAAQE&sclient=img&ei=VgWwZL7FHv6ijuMPwsmhqAM"
response = requests.get(url)
soup = BeautifulSoup(response.content, "html.parser")

image_urls = []
for img in soup.find_all('img'):
    image_url = img.get('src')
    if image_url:
        image_urls.append(image_url)

if not os.path.exists('2'):
    os.makedirs('2')

num_images_downloaded = 0
for i, url in enumerate(image_urls):
    if num_images_downloaded == 20:
        break
    try:
        filename = os.path.join('2', '2{}.jpg'.format(i))
        urllib.request.urlretrieve(url, filename)
        print('Downloaded', url)
        num_images_downloaded += 1
    except Exception as e:
        print('Failed to download', url, e)
