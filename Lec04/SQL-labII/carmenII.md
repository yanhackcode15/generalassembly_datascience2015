### SQL LAB
# Where In The World Is Carmen Sandiego?, Part II


	Good Work, Gumshoe! Interpol gives you its greatest thanks.

	As we speak, agents are swarming the [location redacted], 
	intercepting Carmen Sandiego on her flight from [location redacted].

	... 

	But wait! Carmen Sandiego has foiled us again!


We're going to use what we've learned already about SQL aggregate functions, grouping, and JOINing tables, and apply it – to chase down and capture an elusive and world-reknowned thief.

Follow the clues, **write down both the commands you used and the actual answers.**

And figure out where Carmen's headed, so we can catch her and bring her in.


## The Chase

**Clue #1:** Someone fitting Carmen Sandiego's description just passed through the country with the highest life expectancy in the world. Which **continent** is she hiding in?

```SQL
-- YOUR SQL QUERY GOES HERE
```

**Clue #2:** Interpol got word that Carmen is hiding in a country that has bigger problems than apprehending her. **On her current continent**, which **country**'s GNP decreased the most since last year? (Double check that the GDP this year is less than the GDP last year.)

```SQL
-- YOUR SQL QUERY GOES HERE
```

**Clue #3:** In the country she is currently in, Carmen Sandiego wants to blend in. So, Carmen must be hiding in the **district** with the highest total population. 

Each city belongs to a district. So, for each district, sum the populations of each city in that district and find the district with the highest population. You already know which country Carmen is in -- now which **district** is she in? (Confirm you are on the right track -- the district contains exactly 2 cities!)

```SQL
-- YOUR SQL QUERY GOES HERE
```

**Clue #4:** Immediately once you discover the district, agents flood the area to put Carmen Sandiego behind bars once and for all. Agents surround a suspicious ACME van, but ... again she eludes them! She's getting cocky again -- too cocky for her own good:

```
	Where I am now, I am finally content.
	Oh boy, and I didn't even leave the continent!
	
	I am relaxing in a place
	Where the district, city, and country are all the same.
	If you do not find me,
	You have only yourself to blame!
```

It's up to you, Gumshoe! Interpol is ready to spring on your word -- which **city** is Carmen Sandiego hiding in? **WHERE IN THE WORLD IS CARMEN SANDIEGO?**

```SQL
-- YOUR SQL QUERY GOES HERE
```


###Follow-Up Challenges

* **Clue #1.** Which continent has the highest average life expectancy? (Hint: use AVG)
		``` -- YOUR SQL QUERY GOES HERE ```

* **Clue #2.** Which country in the world had the largest **increase** in GNP?
		``` -- YOUR SQL QUERY GOES HERE ```

* **Clue #3.** How many country districts contain exactly 18 cities?
		``` -- YOUR SQL QUERY GOES HERE ```

