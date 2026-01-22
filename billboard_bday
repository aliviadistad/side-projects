import requests
from bs4 import BeautifulSoup
import datetime

def get_chart_date(birthday):

    """Scrapes for the #1 song of the week (that saturday) of the
    users birthday """

    date = datetime.datetime.strptime(birthday, "%Y-%m-%d")
    # Find the Saturday of that week

    days_until_saturday = (5 - date.weekday()) % 7
    chart_date = date + datetime.timedelta(days=days_until_saturday)
    return chart_date.strftime("%Y-%m-%d")

def get_number_one_song(chart_date):
    
    """Scrapes Billboard for the #1 song on the given date"""

    url = f"https://www.billboard.com/charts/hot-100/{chart_date}/"
    response = requests.get(url)

    if response.status_code != 200:
        raise Exception(f"Failed to retrieve data:")
        return None
    soup = BeautifulSoup(response.text, 'html.parser')

    #find the first entry
    first_entry = soup.find('li', class_='o-chart-results-list__item')
    if not first_entry:
        print("Could not find the #1 song on the page.")
        return None
    
    #extract first entry
    song_tag = first_entry.find('h3', class_='c-title')
    artist_tag = first_entry.find('span', class_='c-label')

    if song_tag and artist_tag:
        song_title = song_tag.get_text(strip=True)
        artist_name = artist_tag.get_text(strip=True)
        return song_title, artist_name
    
    print("Could not extract song or artist information.")
    return None

#main program
birthday = input("Enter your birthday (YYYY-MM-DD): ")
chart_date = get_chart_date(birthday)
result = get_number_one_song(chart_date)

if result:
    song , artist = result
    print(f"On {chart_date}, the #1 song was '{song}' by {artist}.")
else:
    print("Could not retrieve the #1 song information.")
