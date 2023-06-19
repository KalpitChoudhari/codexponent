---
layout: post
title:  "Testing JSON Jbuilder Responses in Rails Controllers"
author: Kalpit
categories: [ Ruby, Rails ]
image: assets/images/jbuilder_2.png
---

## Introduction:
When building Rails applications, it's essential to test the responses of our controllers, especially when generating JSON output using Jbuilder templates. In this article, we'll explore how to effectively test JSON Jbuilder responses for a `Book` model's show action in the `BooksController`.

### Setting up the Scenario:
Consider a **`Book`** model with attributes such as **`id`**, **`title`**, **`author_name`**, and **`genre`**. We have a show action in the **`BooksController`** that fetches a book by its ID.

```ruby
# == Schema Information
#
# Table name: books
#
#  id          :bigint           not null, primary key
#  author_name :string
#  genere      :string
#  title       :string
#  created_at  :datetime         not null
#  updated_at  :datetime         not null
#
class Book < ApplicationRecord
end
```

```ruby
class BooksController < ApplicationController
  def show
    @book = Book.find_by(id: params[:id])
  end
end
```

## Writing the Jbuilder Template:
To generate the JSON response, we use a Jbuilder template for the show action. The template specifies the attributes to include in the response, such as **`id`**, **`title`**, **`author_name`**, and **`genre`**.

```ruby
json.id @book.id
json.title @book.title
json.author_name @book.author_name
json.genere @book.genere

# Sample output of url: /books/:id could be:

{
    "id": 1,
    "title": "The Fault in Our Stars",
    "author_name": "John Green",
    "genere": "Fiction"
}
```

### Testing the JSON Jbuilder Response:

To test the JSON response, we need to write controller tests using RSpec. By including the **`render_views`** directive, we can render the response from the Jbuilder template.

In the test case, we make a **`GET`** request to the show action, passing the book's ID and specifying the response format as JSON. We then parse the response body using **`JSON.parse`** and can assert against the expected values.

```ruby
require 'rails_helper'

RSpec.describe BooksController, type: :controller do
  render_views

  describe "GET show" do
    it "returns a JSON response with the attributes specified in the Jbuilder serializer" do
      book = Book.create(title: "The Fault in Our Stars", author_name: "John Green", genre: "Fiction")

      get :show, params: { id: book.id }, format: :json

      parsed_body = JSON.parse(response.body)
      expect(parsed_body["id"]).to eq(book.id)
      expect(parsed_body["title"]).to eq(book.title)
      expect(parsed_body["author_name"]).to eq(book.author_name)
      expect(parsed_body["genre"]).to eq(book.genre)
    end
  end
end
```

By executing this test, we can ensure that the JSON response from the show action contains the expected attributes and values as specified in the Jbuilder template.
