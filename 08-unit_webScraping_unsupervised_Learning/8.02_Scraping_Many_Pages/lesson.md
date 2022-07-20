# Lesson 8.2 Web scraping extended - Scraping multiple pages

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to take their web scraping skills one step further, by scraping multiple pages.

---

### Learning Objectives:

After this lesson, students will be able to:

- Engage in a complex web scraping task involving scraping multiple pages

---

### Teacher material:

To prepare and give this lesson, you can use:

- The [dataquest tutorial](https://www.dataquest.io/blog/web-scraping-beautifulsoup/)

:exclamation: Note for instructor: This lesson and the next one are very practical and build up directly on the previous lessons. Instead of the lesson-activity format, the whole lesson is like an activity: we define the objective and give them some time to solve the problem for themselves, then review the solution in a code along: web scraping is much more about practicing with many examples than about concepts. **This being said, there are no activities per-se, before coding the solution - define the requirements to students, let them work on solutions for some time and then do it together.**

---

### Lesson 1 key concepts

> :clock10: 20 min

- Understand how to assemble URLs to send multiple requests
- Use `sleep()` to be respectful when scraping and not get banned

<details>
<summary> Web scraping multiple pages </summary>

We practiced web scraping when all the information is in a single table of a single page in a site. What happens when we want to scrape information from multiple pages?

- Go to [https://www.imdb.com/search/title/](https://www.imdb.com/search/title/) and enter the following parameters, leaving all other fields blank or with its default value:

  - Title Type: Feature film
  - Release date: From 1990 to 1992 (You don't need to enter the month and/or date here)
  - User Rating: 7.5 to -

The page you get should be familiar. There's a list with movies and each movie has its title, release year, crew, etc. You could inspect the page and build the code to collect the date.
However, the results we obtained contain 631 movies, and each page only contains 50 of them (you can change the settings to obtain up to 250 movies/page, but that still won't make it till the end).
The way to automatize web scraping in these cases is to look at the URLs The one we've obtained is the following: `https://www.imdb.com/search/title/?title_type=feature&release_date=1990-01-01,1992-12-31&user_rating=7.5,`

If you scroll down and click on "Next", the URL is now:
`https://www.imdb.com/search/title/?title_type=feature&release_date=1990-01-01,1992-12-31&user_rating=7.5,&start=51&ref_=adv_nxt`

Click again on "Next" and here's the new URL:
`https://www.imdb.com/search/title/?title_type=feature&release_date=1990-01-01,1992-12-31&user_rating=7.5,&start=101&ref_=adv_nxt`

The patterns are clear: our search options are in the parameters `title_type`, `release_date`and `user_rating`. Then, we have the `start` parameter, which jumps in intervals of 50, and the `ref_` parameter, which takes the value of "adv_nxt".

Let's do some requests:

```python
# 1. import libraries
from bs4 import BeautifulSoup
import requests

# 2. url: we start with the 'second' page
url = "https://www.imdb.com/search/title/?title_type=feature&release_date=1990-01-01,1992-12-31&user_rating=7.5,&start=51&ref_=adv_nxt"

# 3. download html with a get request
response = requests.get(url)
response.status_code # 200 status code means OK!

# 4.1. parse html (create the 'soup')
soup = BeautifulSoup(response.content, "html.parser")
# 4.2. check that the html code looks like it should
soup

```

Now, we'll have to build a loop where we simply replace the 51 for all the other values (jumping by 50) up until the end of the results. For simplicity, we will build manually this list of values to iterate through:

```python
iterations = range(1, 631, 50)

for i in iterations:
    start_at= str(i)
    url = "https://www.imdb.com/search/title/?title_type=feature&release_date=1990-01-01,1992-12-31&user_rating=7.5,&start=" + start_at + "&ref_=adv_nxt"
    print(url)
```

**Respectful scraping:**

There we have it, all the URLs we need! Before starting with the actual scraping, though, there's something we need to note when sending massive, automated requests to websites: it's rude.
We just have 13 of them, which is not too many, but it's still a good practice to let a few seconds pass in between requests. Some pages don't like being scraped and will block your IP if they detect it's sending automated requests. Others might have a small server for the traffic they handle, and sending too many requests might crash the site.
The sleep module will help us with that. Here's how it works, waiting 2 seconds between each iteration in a for loop:

```python
from time import sleep
for i in range(5):
    print(i)
    sleep(2)
```

To make it more "human", we can randomize the waiting time:

```python
from random import randint
for i in range(5):
    print(i)
    wait_time = randint(1,4)
    print("I will sleep for " + str(wait_time) + " seconds.")
    sleep(wait_time)
```

</details>

<details>
  <summary> Assembling the script to send and store multiple requests </summary>

We have already covered all the pieces leading to the code to respectfully send multiple requests but it's still good to go slowly over the full script below with the students:

```python
pages = []

for i in iterations:
    # assemble the url:
    start_at= str(i)
    url = "https://www.imdb.com/search/title/?title_type=feature&release_date=1990-01-01,1992-12-31&user_rating=7.5,&start=" + start_at + "&ref_=adv_nxt"

    # download html with a get request:
    response = requests.get(url)

    # monitor the process by printing the status code
    print("Status code: " + str(response.status_code))

    # store response into "pages" list
    pages.append(response)

    # respectful nap:
    wait_time = randint(1,4)
    print("I will sleep for " + str(wait_time) + " second/s.")
    sleep(wait_time)
```

Note how if you print the object `pages` after running the code above, you'll just see the response code messages, but the html code is still accessible and you can parse it the same way we've always done:

```python
BeautifulSoup(pages[0].content, "html.parser")
```

It's also worth noting that we're storing much more data than we need. We could filter the information in the loop.

</details>

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 15 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>

- No activity, students practiced with the previous codealong.

</details>

---

:coffee: **BREAK**

---

### Lesson 2 key concepts

> :clock10: 20 min

It's the moment to build the code that collects all the 631 movie titles and their synopsis in a dataframe.

The full code to do that is below, but try to go with students one step at a time, and keep showing them, using Chrome Dev Tools, how you can locate the elements (copy the Selector like we did in the previous lesson, and then trim it as much as possible).

For example, one intermediate step would be to locate the titles:

```python
# Parse just the first page, for testing purposes
soup = BeautifulSoup(pages[0].content, "html.parser")

# Paste the Selector from the first movie title copied from Chrome Dev Tools
soup.select("#main > div > div.lister.list.detail.sub-list > div > div:nth-child(1) > div.lister-item-content > h3 > a")

# Trim the selection: now it grabs all the titles
soup.select("div.lister-item-content > h3 > a")
```


You can do the same for the synopsis:

```python
# Paste the Selector from the first movie title copied from Chrome Dev Tools
soup.select("#main > div > div.lister.list.detail.sub-list > div > div:nth-child(1) > div.lister-item-content > p:nth-child(4)")

# Trim the selection: now it grabs all the titles
soup.select("div.lister-item-content > p:nth-child(4)")
```


One of the ugliest things about the code above is that the HTML element containing the synopsis does not have any combination of tag and attribute that makes it unique. We've had to use `select("p:nth-child(4)")` and simply grab the 4th `<p>` element. Not very elegant... potentially will break... but, for now, it works.

We have noticed how both the title and the synopsis are children of `div.lister-item-content`. That will make our looping task a bit simpler.

There are many approaches to do this. The one we'll follow is:

- Loop through the pages we collected, parse them ("create the soup") and store the parsed pages in a list.
- For each parsed page, select the "blocks of HTML elements" that contain all the information of each movie (the title, the synopsis and other stuff).
- For each one of the "blocks" we collected in the previous step:
  - Get the movie titles and store them in a list
  - Get the synopsis and store them in a list

Here's the code that does that:

```python
pages_parsed = []
titles = []
synopsis = []

for i in range(len(pages)):
    # parse all pages
    pages_parsed.append(BeautifulSoup(pages[i].content, "html.parser"))
    # select only the info about the movies
    movies_html = pages_parsed[i].select("div.lister-item-content")
    # for movie, store titles and reviews into lists
    for j in range(len(movies_html)):
        titles.append(movies_html[j].select("h3 > a")[0].get_text())
        synopsis.append(movies_html[j].select("p:nth-child(4)")[0].get_text().strip())

# Checking our output:

print(len(titles)) # output: 631
print(len(synopsis))  # output: 631

# Note: in your output the movie titles might be in English:
titles[0:3] # output: ['El silencio de los corderos', 'Uno de los nuestros', 'Solo en casa']
synopsis[0:3] #output: ['A young F.B.I. cadet must receive the help of an incarcerated and manipulative cannibal killer to help catch another serial killer, a madman who skins his victims.', 'The story of Henry Hill and his life in the mob, covering his relationship with his wife Karen Hill and his mob partners Jimmy Conway and Tommy DeVito in the Italian-American crime syndicate.', 'An eight-year-old troublemaker must protect his house from a pair of burglars when he is accidentally left home alone by his family during Christmas vacation.']
```

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 20 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

- No activity, students practiced with the previous codealong.

</details>

<details>
  <summary> Activity 2 solutions</summary>

</details>

---

:coffee: **BREAK**

---

### Lesson 3 key concepts

> :clock10: 30 min

- Finding multiple links, following them and scraping stuff from each page.

:exclamation: Note for instructor: This lesson and the next one are very practical and build up directly on the previous lessons. Instead of the lesson-activity format, the whole lesson is like an activity: we define the objective and give them some time to solve the problem for themselves, then review the solution in a code along: web scraping is much more about practicing with many examples than about concepts.

<details>
<summary> Defining the task </summary>

Our objective is to create a dataframe with information about the presidents of the United States. To do this, we will go through this steps:

1. Scrape this [list of presidents of the United States](https://en.wikipedia.org/wiki/List_of_presidents_of_the_United_States).

2. Collect all the links to the Wikipedia page of each president.

3. Scrape the Wikipedia page of each president.

4. Find and store information about each president.

5. Organize the information in a dataframe where we have each president as a row and each variable we collected as a column.

<summary> Scraping links from a Wikipedia table </summary>

```python
# 1. import libraries
from bs4 import BeautifulSoup
import requests
import pandas as pd


# 2. find url and store it in a variable
url = "https://en.wikipedia.org/wiki/List_of_presidents_of_the_United_States"

# 3. download html with a get request
response = requests.get(url)
response.status_code # 200 status code means OK!

# 4.1. parse html (create the 'soup')
soup = BeautifulSoup(response.content, "html.parser")
# 4.2. check that the html code looks like it should
soup

# this solution is not very elegant, but works. The CSS selector we copied had an "nth-child" that we could iterate to find presidents, but some elements were empty, so we concatenate each new element with "+" instead of appending as usual:

presidents = []

for i in range(95):
    presidents = presidents + soup.select("tbody > tr:nth-child(" + str(i) + ") > td:nth-child(4) > b > a")

# check the output:
presidents
```

<details>
  <summary> Expected output of "presidents" </summary>

```html
[<a href="/wiki/George_Washington" title="George Washington">George Washington</a>,
<a href="/wiki/John_Adams" title="John Adams">John Adams</a>,
<a href="/wiki/Thomas_Jefferson" title="Thomas Jefferson">Thomas Jefferson</a>,
<a href="/wiki/James_Madison" title="James Madison">James Madison</a>,
<a href="/wiki/James_Monroe" title="James Monroe">James Monroe</a>,
<a href="/wiki/John_Quincy_Adams" title="John Quincy Adams">John Quincy Adams</a>,
<a href="/wiki/Andrew_Jackson" title="Andrew Jackson">Andrew Jackson</a>,
<a href="/wiki/Martin_Van_Buren" title="Martin Van Buren">Martin Van Buren</a>,
<a href="/wiki/William_Henry_Harrison" title="William Henry Harrison">William Henry Harrison</a>,
<a href="/wiki/John_Tyler" title="John Tyler">John Tyler</a>,
<a href="/wiki/James_K._Polk" title="James K. Polk">James K. Polk</a>,
<a href="/wiki/Zachary_Taylor" title="Zachary Taylor">Zachary Taylor</a>,
<a href="/wiki/Millard_Fillmore" title="Millard Fillmore">Millard Fillmore</a>,
<a href="/wiki/Franklin_Pierce" title="Franklin Pierce">Franklin Pierce</a>,
<a href="/wiki/James_Buchanan" title="James Buchanan">James Buchanan</a>,
<a href="/wiki/Abraham_Lincoln" title="Abraham Lincoln">Abraham Lincoln</a>,
<a href="/wiki/Andrew_Johnson" title="Andrew Johnson">Andrew Johnson</a>,
<a href="/wiki/Ulysses_S._Grant" title="Ulysses S. Grant">Ulysses S. Grant</a>,
<a href="/wiki/Rutherford_B._Hayes" title="Rutherford B. Hayes">Rutherford B. Hayes</a>,
<a href="/wiki/James_A._Garfield" title="James A. Garfield">James A. Garfield</a>,
<a href="/wiki/Chester_A._Arthur" title="Chester A. Arthur">Chester A. Arthur</a>,
<a href="/wiki/Grover_Cleveland" title="Grover Cleveland">Grover Cleveland</a>,
<a href="/wiki/Benjamin_Harrison" title="Benjamin Harrison">Benjamin Harrison</a>,
<a href="/wiki/Grover_Cleveland" title="Grover Cleveland">Grover Cleveland</a>,
<a href="/wiki/William_McKinley" title="William McKinley">William McKinley</a>,
<a href="/wiki/Theodore_Roosevelt" title="Theodore Roosevelt">Theodore Roosevelt</a>,
<a href="/wiki/William_Howard_Taft" title="William Howard Taft">William Howard Taft</a>,
<a href="/wiki/Woodrow_Wilson" title="Woodrow Wilson">Woodrow Wilson</a>,
<a href="/wiki/Warren_G._Harding" title="Warren G. Harding">Warren G. Harding</a>,
<a href="/wiki/Calvin_Coolidge" title="Calvin Coolidge">Calvin Coolidge</a>,
<a href="/wiki/Herbert_Hoover" title="Herbert Hoover">Herbert Hoover</a>,
<a href="/wiki/Franklin_D._Roosevelt" title="Franklin D. Roosevelt">Franklin D. Roosevelt</a>,
<a href="/wiki/Harry_S._Truman" title="Harry S. Truman">Harry S. Truman</a>,
<a href="/wiki/Dwight_D._Eisenhower" title="Dwight D. Eisenhower">Dwight D. Eisenhower</a>,
<a href="/wiki/John_F._Kennedy" title="John F. Kennedy">John F. Kennedy</a>,
<a href="/wiki/Lyndon_B._Johnson" title="Lyndon B. Johnson">Lyndon B. Johnson</a>,
<a href="/wiki/Richard_Nixon" title="Richard Nixon">Richard Nixon</a>,
<a href="/wiki/Gerald_Ford" title="Gerald Ford">Gerald Ford</a>,
<a href="/wiki/Jimmy_Carter" title="Jimmy Carter">Jimmy Carter</a>,
<a href="/wiki/Ronald_Reagan" title="Ronald Reagan">Ronald Reagan</a>,
<a href="/wiki/George_H._W._Bush" title="George H. W. Bush">George H. W. Bush</a>,
<a href="/wiki/Bill_Clinton" title="Bill Clinton">Bill Clinton</a>,
<a href="/wiki/George_W._Bush" title="George W. Bush">George W. Bush</a>,
<a href="/wiki/Barack_Obama" title="Barack Obama">Barack Obama</a>,
<a href="/wiki/Donald_Trump" title="Donald Trump">Donald Trump</a>]
```

</details>
<summary> Extracting the infobox table for a single president </summary>

When trying to scrape information from many "similar" pages, it's a good idea to start with a couple of these pages individually, to quickly test the code.

Below, we're scraping the right panel with information (`infobox`) on the Wikipedia page of a president:

```python
# send request
url = "https://en.wikipedia.org/" + presidents[0]["href"]
response = requests.get(url)
response.status_code

# parse & store html
soup = BeautifulSoup(response.content, "html.parser")
soup.find("table", { "class" : "infobox vcard"})
```

</details>

Once we managed to make this code work and we tested it for a few presidents, it's time to create the loop that will send all the requests.

In this step we could very well store the whole wikipedia page for each president, or just the tiny, final pieces of information. Storing the boxes is a middle ground (we don't have too much noise but retain the flexibility of deciding later which specific elements to extract).

When sending multiple requests, remember to be respectful by spacing the requests a few seconds from each other. We will also print the success code to monitor that everything is going well:

```python
# 2. find url and store it in a variable

presi_soups = []

for presi in presidents:
    # send request
    url = "https://en.wikipedia.org/" + presi["href"]
    response = requests.get(url)
    print(presi.get_text(), response.status_code)

    # parse & store html
    soup = BeautifulSoup(response.content, "html.parser")
    presi_soups.append(soup.find("table", {"class":"infobox vcard"}))

    # respectful nap:
    wait_time = randint(1,4)
    print("I will sleep for " + str(wait_time) + " second/s.")
    sleep(wait_time)
```

---

</details>

---

:coffee: **BREAK**

---

### Lesson 4 key concepts

> :clock10: 20 min

- We follow the task from the previous lesson, with the exact same methodology. The objective now is to find and extract as much information as possible for each president. Students will soon realize how difficult it is to create code that works with each one of the 45 pages - and will have to use error handling.

<details>
  <summary> Finding personal details for a single president </summary>

We extracted the `infobox`es: now it's time to extract specific pieces of information from them. Let's test what can we get from single presidents and then assemble a loop for all of them - as usual.

Here, we will use [the string argument](https://www.crummy.com/software/BeautifulSoup/bs4/doc/#the-string-argument) in the find function, since wikipedia tags and classes are not always helpful to locate. The string argument allows us to locate elements by its actual content.

```python
#Birthday
presi_soups[-1].find("span", {"class":"bday"}).get_text()

#Political party
presi_soups[-1].find("th", string="Political party").parent.find("a").get_text()

#Number of sons/daughters
len(presi_soups[-1].find("th", string="Children").parent.find_all("li"))
```

Let's see if we can extract these items for all of the presidents:

```python
birthdays = []
parties = []
children = []

for presi in presi_soups:
    birthdays.append(presi.find("span", {"class":"bday"}).get_text())
    parties.append(presi.find("th", string="Political party").parent.find("a").get_text())
    #children.append(len(presi.find("th", string="Children").parent.find_all("li")))
```

This code gives an error - looks like not all presidents have children, and that this information is stored with different html tags depending on the president.

As a patch to this problem, we can implement simple error handling:

```python
for i in presi_soups:
    try:
        print(i.find("th", string="Children").parent.find_all("li"))
    except:
        print("NA")
```

Let's put the code together and create a dataframe:

```python
president_name = []
birthdays = []
parties = []
children = []

for presi in presi_soups:
    birthdays.append(presi.find("span", {"class":"bday"}).get_text())
    parties.append(presi.find("th", string="Political party").parent.find("a").get_text())
    president_name.append(presi.find("div",{"class":"fn"}).get_text())
    try:
        children.append(len(presi.find("th", string="Children").parent.find_all("li")))
    except:
        children.append("NA")

presis_df = pd.DataFrame({"name":president_name,
                          "birthday":birthdays,
                          "party": parties,
                          "children": children})
```

</details>

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-web-scraping-multiple-pages](https://github.com/ironhack-labs/lab-web-scraping-multiple-pages)

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

</details>

---

:sandwich: **LUNCH BREAK**

---
