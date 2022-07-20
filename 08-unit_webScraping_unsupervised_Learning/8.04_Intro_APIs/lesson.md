# Lesson 8.4 - Intro to APIs - REST APIs - HTTP Requests

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to introduce the business case study of the music segmentation of a company. In this lesson, we will try to understand what an API is and how can we connect to some of them. We will be working mainly with the `Skyscanner` API, in the next lesson we will start using the `Spotify` API.

---

### Learning Objectives:

After this lesson, students will be able to:

- Understand what an API is
- HTTP Protocol and requests library
- Access to APIs with Python
- JSON format overview
- `Skyscanner` API

---

### Lesson 1 key concepts

> :clock10: 20 min

- Understand what an API is
- RESTful APIs

:exclamation: Note for instructor:For this lesson you can introduce or reinforce how to protect the the keys and passwords to not push to Github (.gitignore).

<details>
<summary> Click for Description: Introduction to Case study </summary>

- Our company _<company_name>_ is interested in grouping clients by their musical interests. To do that we will need to collect data from musical sites so we can understand better this topic. We have heard that the `Spotify` **API** is pretty good for this kind of data collection so we will take a look at it. If it's note enough or we need more information, we will try to use other data collection methods in later lessons (**web scraping**)

</details>

<details>
  <summary>Click for Description: What is an API?</summary>

- An **API** (_Application Programming Interface_) is a type of intermediary software that allows two applications to talk to each other and communicate between them. Every time you use YouTube or Wikipedia, you're also using an API.
- When you use an application it connects to the Internet and sends the required information to a server. The server then retrieves that data, interprets it, performs the necessary actions and calculations so it can send the desired information / action to your phone. The application then interprets the data and presents it to you in a readable way. All of this happens via APIs.
- You can imagine the API as a waiter in a restaurant. The guest (application) was some food (data) and the chef (server) can cook for him (give data to the application). But instead of letting the person ask directly what he wants to the chef (what would make him mental, having all the clients talking to him at the same time) there is a procedure, a helper, in the restaurant there is a waiter that helps with the commands and carry the food from one side to another (app -server). This is exactly what an API is.

</details>
<summary> Click for Description: Why do we use APIs? </summary>

- Sometimes it is better to use a simple CSV file instead, but APIs are useful in the following cases:

  - **The data changes quickly**. Stock price data or betting houses are a perfect example for it. It doesn’t really make sense to regenerate a dataset and download it every second. It is incredibly expensive and wouldn't be efficient nor effective at all, as it would be not just expensive but also really slow.

  - **You want just a piece of all your data**. Imagine you want to download your facebook pictures. Without an API you would need to download the entire Facebook dataset, and that doesn't really make a lot of sense since we have APIs to filter that information.

  - **Repeated computation involved**. The `Spotify API` that can tell you the genre of a piece of music. You could theoretically create your own classifier, and use it to compute music categories, but you’ll never have as much data as `Spotify` does, saving a lot of space.

Overall we can say that the main advantages of using APIS are **Automation**, **Efficiency** and **Adaptation**.

There are many ways in which web services can be organized but the most popular way is to make a REST (**RE**presentational **S**tate **T**ransfer) API.

REST defines 6 architectural constraints which make any web service a RESTful API. You can find them [here](https://www.geeksforgeeks.org/rest-api-architectural-constraints/#:~:text=The%20only%20optional%20constraint%20of,API%20and%20Non-REST%20API.&text=For%20example%3A%20API%2Fusers.). (These concepts can be explained to more advanced classes, added as **optional** content)

This is important because it generalizes the use of **HTTP**, making **requests** to specific **URLs**. It is the tool we will be using when interacting with an API.

Like a python object, HTTP has different methods, including the most important ones **GET** and **POST**. We will take a look at it in the next lesson.

---

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>

- Link to [8.04 Activity 1](https://github.com/ironhack-edu/data_8.04_activities/blob/master/8.04_activity_1.md).

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

- Link to [8.04 Activity 1 solution](https://gist.github.com/ironhack-edu/ba6ee15e4cffed82a83b664dfc5611de).

</details>

---

:coffee: **BREAK**

---

### Lesson 2 key concepts

> :clock10: 20 min

- We will start working with APIs in the next lesson but first we need to get in touch with the tools that will make the connection possible as well as an overview of the **REST**ful way.
- The first tool we will take a look at will be the **requests** library, that's meant to cope with **HTTP** and watch out the status code.
- We will also see some files in **JSON** format to understand its structure and be able to use them

  - Now let's start by installing the request library and importing it as well doing a request get with the following websites: [Google](https://developers.google.com), [NBA](https://api.sportsdata.io/api/nba/fantasy/json/CurrentSeason) and [Rotten Tomatoes](http://api.rottentomatoes.com/api/public/v1.0/lists/movies/box_office.json). Let's check their `status_code` by using the requests method **GET**, which simply indicates that you're trying to retrieve information from the data source:

```python
#Install requests library with the command promt

# Import requests library
import requests

google = requests.get("https://developers.google.com")
print("Google:", google.status_code)

NBA = response = requests.get("https://api.sportsdata.io/api/nba/fantasy/json/CurrentSeason")
print("NBA:", NBA.status_code)

rotten_tomato = requests.get("http://api.rottentomatoes.com/api/public/v1.0/lists/movies/box_office.json")
print("Rotten Tomatoes:", rotten_tomato.status_code)
```

Output:

```python
Google: 200
NBA: 401
Rotten Tomatoes: 403
```

So we can see we receive some **status codes**, the most frequent are these:

- `200`: Everything went okay and the result has been returned (if any).
- `301`: The server is redirecting you to a different endpoint. This can happen when a company switches domain names, or an endpoint name is changed.
- `400`: The server thinks you made a bad request. This happens when you don’t send along the right data, among other things.
- `401`: You are not properly authenticated.
- `403`: The resource you’re trying to access is forbidden: you don’t have the right permissions to get it.
- `404`: The resource you tried to access doesn't exist.
- `503`: The server can't handle the request.
</details>

<details>
  <summary> After connecting </summary>

- As we are connected now to the google developer endpoint let's try to get some data from it

```python
print(google.text)
```

This is not the proper format for us right now so let's try to tranform it to the one we need, the **JSON** (JavaScript Object Notation) format, which is the language of RESTful APIs. It is the way to encode data to make data easily readable by machines.

```python
google.json()
```

Output

```python
JSONDecodeError: Expecting value: line 1 column 1 (char 0)
```

The problem here is that we are not connected to a proper API that has a json as the endpoint, so we will need to get another valid URL. Remember that the code `200`means that the connection has been established, but not that it is the connection you need. Let's try with the `skyscrapper` API .

We will connect to this API through the web [RapidAPI](https://rapidapi.com/skyscanner/api/skyscanner-flight-search) so you will need to create an account to get a personal **Key** (user ID) to have access to it, just sign up and get your Key.
The "problem" with this API is that we will need to pass some **parameters**, including the **headers** and the **city** following this code:

```python
url = "https://skyscanner-skyscanner-flight-search-v1.p.rapidapi.com/apiservices/autosuggest/v1.0/UK/GBP/en-GB/"

params = {"query":"Tokyo"}

headers = {'x-rapidapi-host': "skyscanner-skyscanner-flight-search-v1.p.rapidapi.com",
                      'x-rapidapi-key': "<introduce your RapidAPI key here>"}

response = requests.get(url, headers = headers, params = params)
response.json()
```

Output:

```python
{"Places":[{"PlaceId":"TYOA-sky","PlaceName":"Tokyo","CountryId":"JP-sky","RegionId":"","CityId":"TYOA-sky","CountryName":"Japan"},{"PlaceId":"NRT-sky","PlaceName":"Tokyo Narita","CountryId":"JP-sky","RegionId":"","CityId":"TYOA-sky","CountryName":"Japan"},{"PlaceId":"HND-sky","PlaceName":"Tokyo Haneda","CountryId":"JP-sky","RegionId":"","CityId":"TYOA-sky","CountryName":"Japan"},{"PlaceId":"TJH-sky","PlaceName":"Toyooka","CountryId":"JP-sky","RegionId":"","CityId":"JTJH-sky","CountryName":"Japan"},{"PlaceId":"OOK-sky","PlaceName":"Toksook Bay","CountryId":"US-sky","RegionId":"AK","CityId":"OOKA-sky","CountryName":"United States"},{"PlaceId":"TKZ-sky","PlaceName":"Tokoroa","CountryId":"NZ-sky","RegionId":"","CityId":"TKZN-sky","CountryName":"New Zealand"}]}
```

To know the parameters we need to be able to connect properly to an API we should look first at its documentation. For the next query we will look at the prices for the flights from San Francisco to New York City in 2020/12/12

```python
url = "https://skyscanner-skyscanner-flight-search-v1.p.rapidapi.com/apiservices/browsequotes/v1.0/US/USD/en-US/SFO-sky/NYCA-sky/2020-12-12"

params = {"inboundpartialdate":"2020-12-12"}

headers = {
    'x-rapidapi-host': "skyscanner-skyscanner-flight-search-v1.p.rapidapi.com",
    'x-rapidapi-key': "<introduce your RapidAPI key here>"}

response = requests.get(url, headers=headers, params=params)

response.json()
```

Output:

```python
{'Quotes': [{'QuoteId': 1,
   'MinPrice': 92.0,
   'Direct': False,
   'OutboundLeg': {'CarrierIds': [1065],
    'OriginId': 81727,
    'DestinationId': 50290,
    'DepartureDate': '2020-12-12T00:00:00'},
   'QuoteDateTime': '2020-09-21T10:49:00'},
  {'QuoteId': 2,
   'MinPrice': 133.0,
   'Direct': True,
   'OutboundLeg': {'CarrierIds': [851],
    'OriginId': 81727,
    'DestinationId': 50290,
    'DepartureDate': '2020-12-12T00:00:00'},
   'QuoteDateTime': '2020-09-21T10:49:00'}],
 'Places': [{'PlaceId': 50290,
   'IataCode': 'EWR',
   'Name': 'New York Newark',
   'Type': 'Station',
   'SkyscannerCode': 'EWR',
   'CityName': 'New York',
   'CityId': 'NYCA',
   'CountryName': 'United States'},
  {'PlaceId': 60987,
   'IataCode': 'JFK',
   'Name': 'New York John F. Kennedy',
   'Type': 'Station',
   'SkyscannerCode': 'JFK',
   'CityName': 'New York',
   'CityId': 'NYCA',
   'CountryName': 'United States'},
  {'PlaceId': 65633,
   'IataCode': 'LGA',
   'Name': 'New York LaGuardia',
   'Type': 'Station',
   'SkyscannerCode': 'LGA',
   'CityName': 'New York',
   'CityId': 'NYCA',
   'CountryName': 'United States'},
  {'PlaceId': 81727,
   'IataCode': 'SFO',
   'Name': 'San Francisco International',
   'Type': 'Station',
   'SkyscannerCode': 'SFO',
   'CityName': 'San Francisco',
   'CityId': 'SFOA',
   'CountryName': 'United States'}],
 'Carriers': [{'CarrierId': 819, 'Name': 'Aegean Airlines'},
  {'CarrierId': 851, 'Name': 'Alaska Airlines'},
  {'CarrierId': 870, 'Name': 'jetBlue'},
  {'CarrierId': 1065, 'Name': 'Frontier Airlines'},
  {'CarrierId': 1721, 'Name': 'Sun Country Airlines'},
  {'CarrierId': 1793, 'Name': 'United'},
  {'CarrierId': 1902, 'Name': 'Southwest Airlines'}],
 'Currencies': [{'Code': 'USD',
   'Symbol': '$',
   'ThousandsSeparator': ',',
   'DecimalSeparator': '.',
   'SymbolOnLeft': True,
   'SpaceBetweenAmountAndSymbol': False,
   'RoundingCoefficient': 0,
   'DecimalDigits': 2}]}
```

We will learn what the JSON format really is in the next lesson, for now just try to go with the activity

</details>
#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

- Link to [8.04 Activity 2](https://github.com/ironhack-edu/data_8.04_activities/blob/master/8.04_activity_2.md).

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

- Link to [8.04 Activity 2 solution](https://gist.github.com/ironhack-edu/1f0c7beca2452e203f7c64b6a68f97fc).

</details>

---

:coffee: **BREAK**

---

### Lesson 3 key concepts

> :clock10: 20 min

- What is JSON?
- Understanding JSON structure
- Applying it to the skyscanner API

JSON (_JavaScript Object Notation_) is a standard **text-based** format for representing **structured data** (such as dictionaries) based on JavaScript object syntax. JSON exists as a **string**, something very useful when you want to transmit data across a network.

<details>
<summary> What's the JSON structure? </summary>

- To understand how JSON structure works we will look at the previous output.

```python
{'Quotes': [{'QuoteId': 1,
   'MinPrice': 92.0,
   'Direct': False,
   'OutboundLeg': {'CarrierIds': [1065],
    'OriginId': 81727,
    'DestinationId': 50290,
    'DepartureDate': '2020-12-12T00:00:00'},
   'QuoteDateTime': '2020-09-21T10:49:00'},
  {'QuoteId': 2,
   'MinPrice': 133.0,
   'Direct': True,
   'OutboundLeg': {'CarrierIds': [851],
    'OriginId': 81727,
    'DestinationId': 50290,
    'DepartureDate': '2020-12-12T00:00:00'},
   'QuoteDateTime': '2020-09-21T10:49:00'}],
 'Places': [{'PlaceId': 50290,
   'IataCode': 'EWR',
   'Name': 'New York Newark',
   'Type': 'Station',
   'SkyscannerCode': 'EWR',
   'CityName': 'New York',
   'CityId': 'NYCA',
   'CountryName': 'United States'},
  {'PlaceId': 60987,
   'IataCode': 'JFK',
   'Name': 'New York John F. Kennedy',
   'Type': 'Station',
   'SkyscannerCode': 'JFK',
   'CityName': 'New York',
   'CityId': 'NYCA',
   'CountryName': 'United States'},
  {'PlaceId': 65633,
   'IataCode': 'LGA',
   'Name': 'New York LaGuardia',
   'Type': 'Station',
   'SkyscannerCode': 'LGA',
   'CityName': 'New York',
   'CityId': 'NYCA',
   'CountryName': 'United States'},
  {'PlaceId': 81727,
   'IataCode': 'SFO',
   'Name': 'San Francisco International',
   'Type': 'Station',
   'SkyscannerCode': 'SFO',
   'CityName': 'San Francisco',
   'CityId': 'SFOA',
   'CountryName': 'United States'}],
 'Carriers': [{'CarrierId': 819, 'Name': 'Aegean Airlines'},
  {'CarrierId': 851, 'Name': 'Alaska Airlines'},
  {'CarrierId': 870, 'Name': 'jetBlue'},
  {'CarrierId': 1065, 'Name': 'Frontier Airlines'},
  {'CarrierId': 1721, 'Name': 'Sun Country Airlines'},
  {'CarrierId': 1793, 'Name': 'United'},
  {'CarrierId': 1902, 'Name': 'Southwest Airlines'}],
 'Currencies': [{'Code': 'USD',
   'Symbol': '$',
   'ThousandsSeparator': ',',
   'DecimalSeparator': '.',
   'SymbolOnLeft': True,
   'SpaceBetweenAmountAndSymbol': False,
   'RoundingCoefficient': 0,
   'DecimalDigits': 2}]}
```

As we mentioned before, the structure shares some similarities with a python dictionary, that's due to how the data is structured overall, and at the same time we can think of them about dataframes. For any **key** (or column) of the json / dictionary, there will be one or more **values** (or rows). Let's transform the previous output into a pandas dataframe, for that we will need to import **pandas** and use the method `json_normalize` to automatically transform the file into a DataFrame:

</details>
<details>
  <summary> Click for Code Sample </summary>
</details>

```python
# Import libraries
import pandas as pd
import json

# Normalize the response

pd.json_normalize(response.json())
# If this doesn't work, try to import json_normalize directly from pandas.io.json
```

As an output we will have a DataFrame with 4 columns, **Quotes, Places, Carriers** and **Currencies**, but the values will also be in JSON format. From this we can infere that we are not working with a dataset, but a relational database, and the columns are the datasets we were looking for. Let's transform them into `pd.DataFrames`:

```python
quotes = pd.DataFrame(pd.json_normalize(response.json())["Quotes"][0])
carriers = pd.DataFrame(pd.json_normalize(response.json())["Carriers"][0])
places = pd.DataFrame(pd.json_normalize(response.json())["Places"][0])
currencies = pd.DataFrame(pd.json_normalize(response.json())["Currencies"][0])
```

After reviewing and printing our data we can conclude that quotes are the flights we have for our selected origin and destination (just one in this case), places are the airports we could travel from and to, carriers are the companies usually have the kind of trip we have chosen, and currency is self-explanatory.

However we still have another json in one of the datasets, in **Quotes**, variable `["OutboundLeg"]`, that's due to the same principle we have been working for during this lesson, relational databases. Let's unfold that variable:

```python
flights = pd.DataFrame(pd.DataFrame(pd.json_normalize(response.json())["Quotes"][0])["OutboundLeg"][0])
```

</details>

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Link to [8.04 Activity 3](https://github.com/ironhack-edu/data_8.04_activities/blob/master/8.04_activity_3.md).

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions</summary>

- Link to [8.04 Activity 3 solution](https://gist.github.com/ironhack-edu/864da3dbd912a1f58e81439d9decb78a).

</details>

---

:coffee: **BREAK**

---

### Lesson 4 key concepts

> :clock10: 20 min

- Skyscanner diving

---

Now that we know what JSON is and we have connected to the skyscanner API let's work with it. In this lesson we will create a function for which if we introduce two dates, an origin and a destination we will get the data from the cheapest flight between these two dates.

We will start by creating a function that takes two dates and return a list of dates between the given ones. Luckily, there is a pandas method for that.

```python
def get_dates(start, end):
    return pd.Series(pd.date_range(start, end,freq='d').format())
```

Now that we already have our date function let's create another function that takes the origin, destination and dates as arguments. It will create a dictionary that has every day between the two dates and containts a call to the API with all the information about the days:

```python
def flight_days(origin, destination, start, end):
    dates = get_dates(start, end)
    return {date:flight_prices(origin, destination, date) for date in dates}
```

Now let's create another function that returns the date of the cheapest flight

```python
def lowest_price(origin, destination, start, end):
    flights = flight_days(origin, destination, start, end)
    prices = {date:min([quote["MinPrice"] for quote in flights[date]["Quotes"]]) for date in flights for quote in flights[date]["Quotes"]}
    return min(prices, key = prices.get)
```

The problem with this code is that it just gets the first date as the only one even if there are other days with the same price. Try to fix that in the lab.

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-working-with-api](https://github.com/ironhack-labs/lab-working-with-api)

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

- Link to the [lab solution](https://gist.github.com/ironhack-edu/5e8b755ee713f54e82e831dc7a3817ee).

</details>

---

:sandwich: **LUNCH BREAK**

---
