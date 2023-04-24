---
layout: post
title:  "Efficiently Query Multiple Fields in Rails with a Single Query"
author: Kalpit
categories: [ Ruby, Rails ]
image: assets/images/efficient-query.png
---

When working with Rails ORM, I encountered a challenge where I needed to search for students based on their name, email, or mobile number using a single search field. The issue I faced was that there was no way to differentiate which attribute the user was searching for since they could enter any of the three fields in the same text field. As a result, applying a where clause to the correct attribute became difficult.

So, instead of adding new text fields for each of the attribute, I need to apply where clause in backend only. I’ve tried the following:

```
Student.where('name ILIKE ? OR email ILIKE ? OR mobile_number ILIKE', "%#{query}%", "%#{query}%", "%#{query}%")
```

The problem with this query was that I needed to use the search term query three times, once for each attribute. Additionally, if a user entered a combination of name and email, the query would not work as intended.

To solve this issue, I used the concat_ws function in PostgreSQL, which allows me to concatenate the attributes in a single query. Here is an example:

`concat_ws(' ', attr_one, attr_two, attr_three, ....)`

```
Student.where("concat_ws(' ', name, email, mobile_number) ILIKE ?", "%#{query}%")
```

With concat_ws, I was able to apply the same query on multiple fields and significantly simplify the search process.

🥂 🍻

References:

[http://lortza.github.io/2019/12/18/query_multiple_fields.html](http://lortza.github.io/2019/12/18/query_multiple_fields.html)

StackOverflow: [https://stackoverflow.com/questions/9379884/search-multiple-columns-rails](https://stackoverflow.com/questions/9379884/search-multiple-columns-rails)
