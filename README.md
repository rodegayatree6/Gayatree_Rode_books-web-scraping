import requests
from bs4 import BeautifulSoup
import pandas as pd

url = "https://books.toscrape.com/"
soup = BeautifulSoup(requests.get(url).text, "html.parser")

data = []

for book in soup.select("article.product_pod"):
    title = book.h3.a["title"]
    price = book.select_one(".price_color").text
    rating = book.select_one(".star-rating")["class"][1]

    data.append([title, price, rating])

df = pd.DataFrame(data, columns=["Title","Price","Rating"])

# remove any duplicates just in case
df = df.drop_duplicates()

print(df)
