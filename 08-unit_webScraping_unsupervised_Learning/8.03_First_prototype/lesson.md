# Lesson 8.3 Data assessment & Project definition

### Lesson Duration: 1 hour

**Note to instructor:** This lesson is short, because it leaves a space for students to develop their own project. Your role is to make sure they understand the minimum requirements to achieve, and to encourage them to make something unique.

> Purpose: This lesson fills the necessary intermediate step between data collection and modelling. Students will have to re-define the flowchart of their "recommender system" and create a "prototype" of the interface and clean. It's also the moment to explore and assess the quality of the data they collected.

---

### Learning Objectives:

After this lesson, students will be able to:

- Define the specifics of a project out of business needs and data collected
- Create a python pipeline that interacts with users as well as data sources

### Creating a prototype:

Show students the "1st prototype" slide of [this presentation](https://docs.google.com/presentation/d/1JypC2O7Ek9gXSd5W7HhdxXUqqywzTPMet8uDcrwCCUU/edit?usp=sharing).
The objective of this 1st prototype is to create the Python code that achieves this workflow.
This is going to be the very first MVP (Minimum Viable Product) for the clustering-recommender we pretend to achieve this week.
The purpose of building an MVP is to detect as soon as possible the potential blockers / pain points of the project. It's very possible there are hidden complexities that students will have to deal with. So... let students stumble upon problems on their own! When they show their prototype, you can ask for the following issues:

**User experience:**

- What happens if the user inputs a song that doesn't exist?
- What do we do with songs that have the same name, but a different artist?
- How do we deal with typos?

**Architecture:**

- Do we build the interaction with the user in the same notebook as the web-scraping?
- Where do we store the scraped songs?

**Scheduling / Automation:**

- Should we scrape billboard / wikipedia every time a user sends a request?

**Testing:**

- Does it work when you test it with a real user (a colleague)?

Chances are that more issues will appear, and that not all of them will be solved during this session. But what's important is that the issues have been identified.

Attached, there are notebooks with a simple prototype.
