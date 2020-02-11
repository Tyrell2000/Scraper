'''
import cProfile
import pstats

pr = cProfile.Profile()
pr.enable()
'''

import requests
from bs4 import BeautifulSoup
import re
from pandas import DataFrame


# Stat lists for later use
crossover = []
rating = []
language = []
genre = []
chapters = []
words = []
reviews = []
favorites = []
follows = []
updated = []
published = []
status = []
titles = []
story_id = []
characters = []


# Author stats
authors = []


# Holding/Formatting lists to hold data to grab and/or format story stats
formatted = []
links = []
links_author_profile = []
links2 = []
links23 = []

# Genres to check for later(all available)
genre1 = ['Adventure', 'Angst', 'Crime', 'Drama', 'Family', 'Fantasy', 'Friendship', 'General', 'Horror', 'Humor']
genre2 = ['Hurt', 'Comfort', 'Mystery', 'Parody', 'Poetry', 'Romance', 'Sci-Fi', 'Spiritual', 'Supernatural']
genre3 = ['Suspense', 'Tragedy', 'Western']
genre_check = genre1 + genre2 + genre3

# How many web pages will be scraped(total pages - u)
page_num = int(1)
last_page = int(1000)

# Scrapes all the web pages
while page_num <= last_page:
    headers = {
        'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/50.0.2661.102 Safari/537.36'}
    url = 'https://www.fanfiction.net/Naruto_Crossovers/1402/0/?&srt=1&r=10&p=' + str(page_num)
    page = requests.get(url, headers=headers)
    soup = BeautifulSoup(page.content, 'html.parser')

    for link in soup.find_all('a', href={re.compile('/Naruto_Crossovers/1402/0/')}):
        links23.append(link.get('href'))
    link = links23[-2]
    link = link.split('=')

    # Uncomment below to scrape all stories
    # last_page = int(link[-1])

    dish = soup.find_all('div', class_='z-list zhover zpointer ')
    ingredients = soup.find_all('div', class_='z-indent z-padtop')

    employees = soup.find('div', class_="modal-body")
    possible_characters = employees

    for link in soup.find_all('a', href=re.compile("/u/")):
        links_author_profile.append(link.get('href'))
        author = link.a
        author = link.text
        authors.append(author)

    # grabs links to stories
    for link in soup.find_all('a', class_={'stitle': re.compile("/s/")}):
        links.append(link.get('href'))

    # Grabs all the story data from each page
    for n in range(len(ingredients)):

        # Title of the story
        title = dish[n]
        title = title.a
        title = title.text
        titles.append(title)

        # The story stats
        stats = ingredients[n]
        info = stats.div
        text = info.text
        formatted = text.split(' - ')

        # For later use, searches for keywords and adds them to the specified list
        def assign_stats(stats, keyword, specified_stat_list):
            check = 0
            for stat in stats:

                if keyword in stat:
                    specified_stat_list.append(stat)
                    check = 1

                elif stat is stats[-1] and check != 1:
                    specified_stat_list.append('')


        # For later use, Searches for the genre(s) listed and adds them to the specified list
        def assign_stat_genre(stats):

            for stat in stats:

                if "/" in stat:
                    newstat = stat.split("/")
                    if newstat[0] in genre_check and newstat[1] in genre_check:
                        return stat

                if stat in genre_check:
                    return stat

        def assign_stat_characters(stats):

            if not "Published: " in stats[-1]:
                if 'Complete' in stats[-1] and not 'Published:' in stats[-2]:
                    return stats[-2]
                else:
                    if not 'Complete' in stats[-1] and not 'Published' in stats[-1]:
                        return stats[-1]
                    else:
                        return None

        # For later use, searches for keywords and adds them to the specified list
        def assign_stat_status(stats):
            for stat in stats:
                if 'Complete' in stat:
                    status.append(stat)
                else:
                    if stat in stats[-1]:
                        status.append('In-Progress')


        # For later use, searches for specified indexes of story data lists and adds them to specified list
        def assign_stats_concrete(index, specific_stat_list):
            specific_stat_list.append(formatted[index])

        # For later use, searches for keywords and adds them to the specified list
        def assign_stat_language(stats):
            count = 0
            for stat in stats:
                if 'Rated: ' in stat:
                    count = count + 1
                    language.append(stats[count])

                count = count + 1

        # Searches for keywords/indexes for the specified story stat lists
        # assign_stats_concrete('Rated', rating)
        assign_stat_language(formatted)
        assign_stats(formatted, 'Words', words)
        assign_stats(formatted, 'Chapters', chapters)
        assign_stats(formatted, 'Reviews', reviews)
        assign_stats(formatted, 'Favs', favorites)
        assign_stats(formatted, 'Follows', follows)
        assign_stats(formatted, 'Updated', updated)
        assign_stats(formatted, 'Published', published)
        assign_stats(formatted, 'Rated', rating)
        assign_stat_status(formatted)
        assign_stats_concrete(1, crossover)

        list_index = 0
        genre_added = len(genre) + 1

        if len(genre) < (len(titles)):
            getgenre = assign_stat_genre(formatted)
            if getgenre is not None:
                genre.append(getgenre)
            else:
                genre.append('')
            # print('after', list_index)

        getcharacters = assign_stat_characters(formatted)
        if getcharacters is not None:
            characters.append(getcharacters)
        else:
            characters.append('')

    # Prints the current page number
    print('Page', page_num, 'scraped')

    # Goes to the next page, if there is another one
    page_num = page_num + 1

    # Pauses program for between 1 and 4 seconds, keeping to Fanfiction.net TOS
    # time.sleep(randint(1, 4))


for n in range(len(links)):
    dish1 = links[n].split('/')
    story_id.append(dish1[2])


def remove_null(stat_list):
    for i in range(len(stat_list)):
        if stat_list[i] == '':
            stat_list[i] = 0


def str_to_int_conversion(stat_list):
    for i in range(len(stat_list)):
        stat_list[i] = int(stat_list[i])


def formatting(specific_stat_list, keyword):
    count = 0
    for items in specific_stat_list:
        if items is not None:
            specific_stat_list[count] = specific_stat_list[count].replace(keyword, '')
        count = count + 1


formatting(rating, 'Rated: ')
formatting(chapters, 'Chapters: ')
formatting(words, 'Words: ')
formatting(reviews, 'Reviews: ')
formatting(favorites, 'Favs: ')
formatting(follows, 'Follows: ')
formatting(updated, 'Updated: ')
formatting(published, 'Published: ')

for i in range(len(story_id)):
    print(titles[i], ', \tby:', authors[i], '\nRated:', rating[i], ', Chapters:', chapters[i], ', Words:', words[i],
          ', Published:', published[i], ', Status:', status[i], '\nCharacter(s):', characters[i], '\n')


for i in range(len(updated)):
    if updated[i] == '':
        updated[i] = 'Never'


for i in range(len(characters)):
    if characters[i] == '':
        characters[i] = 'Unknown'


formatting(reviews, ',')
formatting(words, ',')
formatting(favorites, ',')
formatting(follows, ',')

remove_null(reviews)
remove_null(favorites)
remove_null(follows)

str_to_int_conversion(story_id)
str_to_int_conversion(chapters)
str_to_int_conversion(words)
str_to_int_conversion(reviews)
str_to_int_conversion(favorites)
str_to_int_conversion(follows)

finalresults = DataFrame({'StoryID': story_id, 'Title': titles, 'Crossover': crossover, 'Genre': genre,
                          'Language': language, 'Author': authors, 'Rating': rating, 'Chapters': chapters,
                          'Words': words, 'Reviews': reviews, 'Favorites': favorites, 'Follows': follows,
                          'Published': published, 'Updated': updated, 'Status': status, 'Characters': characters})

finalresults.to_excel('FanfictionData.xlsx', sheet_name='StoryData')

'''
print(story_id)
print(len(story_id))
print(links)
print(len(links))
print(genre)
print(len(genre))
print(authors)
print(len(authors))
print(words)
print(len(words))
print(rating)
print(len(rating))
print(language)
print(len(language))
print(chapters)
print(len(chapters))
print(reviews)
print(len(reviews))
print(favorites)
print(len(favorites))
print(follows)
print(len(follows))
print(updated)
print(len(updated))
print(status)
print(len(status))
print(published)
print(len(published))
print(crossover)
print(len(crossover))
print(titles)
print(len(titles))
print(characters)
print(len(characters))
'''
'''
pr.disable()
s = io.StringIO()
sortby = 'cumulative'
ps = pstats.Stats(pr, stream=s).sort_stats(sortby)
ps.print_stats()
print(s.getvalue())
'''
