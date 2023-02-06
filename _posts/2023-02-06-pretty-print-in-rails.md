---
layout: post
title:  "Rails console: Pretty Print"
author: Kalpit
categories: [ Ruby, Rails ]
image: assets/images/pretty_print.png
---

Output of rails console is in raw format. Oftentimes, when debugging or using rails console makes us hard to debug the code or print data.

There are couple of ways to pretty print out data in rails console. In the blog we are discussing following of them:

1. Awesome Print
2. Table Print
3. Irbtools

Before moving to the solution, let’s first see how the console prints the data:

```ruby
3.0.3 :059 > data
 => 
[{:id=>1,                           
  :name=>"Geoffrey Keeling",        
  :phone_number=>"+327242979462",   
  :address=>{:city=>"Port Shennaside", :street=>"57958 Austin Trafficway", :secondary_address=>"Apt. 992"}},
 {:id=>2,                           
  :name=>"Eula Schultz",            
  :phone_number=>"+2901772906696",  
  :address=>{:city=>"Fatimaborough", :street=>"696 Tyrone Ranch", :secondary_address=>"Suite 128"}},
 {:id=>3,                           
  :name=>"Sheena Quigley I",        
  :phone_number=>"+17843338063184", 
  :address=>{:city=>"Alonaberg", :street=>"43258 Darius Grove", :secondary_address=>"Suite 870"}},
 {:id=>4,
  :name=>"Johnson Haag CPA",
  :phone_number=>"+18687222826093",
  :address=>{:city=>"New Ian", :street=>"31417 Kalyn Stream", :secondary_address=>"Apt. 885"}},
 {:id=>5,
  :name=>"Pres. Anibal Lynch",
  :phone_number=>"+9655890433140",
  :address=>{:city=>"South Bellatown", :street=>"238 Aracely Trail", :secondary_address=>"Apt. 534"}}]
```

This is randomly generated users data. It looks very messy if we first look at it. So let’s make is more readable.


### Awesome Print

> gem 'awesome_print’

After adding above line into your `gemfile`, next step is to `bundle install` and let’s get back to our console and type `ap DATA_TO_PRETY_PRINT`

```ruby
3.0.3 :060 > ap data
[
    [0] {                              
                  :id => 1,            
                :name => "Geoffrey Keeling",
        :phone_number => "+327242979462",
             :address => {             
                         :city => "Port Shennaside",
                       :street => "57958 Austin Trafficway",
            :secondary_address => "Apt. 992"
        }                              
    },                                 
    [1] {                              
                  :id => 2,            
                :name => "Eula Schultz",
        :phone_number => "+2901772906696",
             :address => {
                         :city => "Fatimaborough",
                       :street => "696 Tyrone Ranch",
            :secondary_address => "Suite 128"
        }
    },
    .....
]
```

### Table Print

As it’s name suggests, it formats data in table form. We can still print our randomly generated dat a with table print.

> gem ‘table_print’

```ruby
3.0.3 :019 > tp data
ID | NAME               | PHONE_NUMBER    | ADDRESS                       
---|--------------------|-----------------|-------------------------------
1  | Pres. Guy Johnston | +17848607085653 | {:city=>"Tempiefort", :stre...
2  | Billy Stracke      | +8213661929040  | {:city=>"East Lashondabury"...
3  | Ilana Crooks       | +2215481175128  | {:city=>"Lake Gwen", :stree...
4  | Normand Collier II | +6905724391042  | {:city=>"Smithport", :stree...
5  | Marta Romaguera    | +18693941040185 | {:city=>"Lake Mikel", :stre...
```

Alternatively we can print out database row in prettier format as:

```ruby
3.0.3 :001 > tp User.limit(5)
  User Load (0.5ms)  SELECT "users".* FROM "users" LIMIT $1  [["LIMIT", 5]]
ID | EMAIL                          | PASSWORD         | NAME                | TYPE   | CREATED_AT              | UPDATED_AT             
---|--------------------------------|------------------|---------------------|--------|-------------------------|------------------------
1  | two.com                        | two              | two                 | simple | 2023-01-07 19:11:03     | 2023-01-07 19:11:03    
2  | micah@fisher.co                | ucBmTzrkoY       | Curt Mertz Ret.     | simple | 2023-02-04 17:14:59     | 2023-02-04 17:14:59    
3  | junko@reilly-kozey.co          | xaFMWuYrNChtn    | Fr. Prince Ziemann  | simple | 2023-02-04 17:14:59     | 2023-02-04 17:14:59    
4  | winnie.gleason@mills-jakubo... | JbzDJknX         | Rosana Thompson Jr. | simple | 2023-02-04 17:14:59     | 2023-02-04 17:14:59    
5  | rosalie_kertzmann@beatty.com   | lNRPCHnThiktMEpO | Cameron Sawayn      | simple | 2023-02-04 17:14:59     | 2023-02-04 17:14:59
```

### Irbtools

> gem ‘irbtools’

Adding `irbtools` in your `gemfile` will print ActiveRecord query output in tabular format. Note that, it print data in tabular format as well as existing console format.

```ruby
3.0.3 :022 > User.limit(2)
  User Load (0.6ms)  SELECT "users".* FROM "users" LIMIT $1  [["LIMIT", 2]]

--------------------------------------------------------------------------------------------------------------------
| id | email           | password   | name            | type   | created_at              | updated_at               |
--------------------------------------------------------------------------------------------------------------------
| 1  | two.com         | two        | two             | simple | 2023-01-07 19:11:03 UTC | 2023-01-07 19:11:03 UTC  |
| 2  | micah@fisher.co | ucBmTzrkoY | Curt Mertz Ret. | simple | 2023-02-04 17:14:59 UTC | 2023-02-04 17:14:59 UTC  |
--------------------------------------------------------------------------------------------------------------------
2 rows in set
=> [#<User:0x00000001191126b8
  id: 1,
  email: "two.com",
  password: "[FILTERED]",
  name: "two",
  type: "simple",
  created_at: Sat, 07 Jan 2023 19:11:03.040433000 UTC +00:00,
  updated_at: Sat, 07 Jan 2023 19:11:03.040433000 UTC +00:00>,
 #<User:0x00000001191125f0
  id: 2,
  email: "micah@fisher.co",
  password: "[FILTERED]",
  name: "Curt Mertz Ret.",
  type: "simple",
  created_at: Sat, 04 Feb 2023 17:14:59.737597000 UTC +00:00,
  updated_at: Sat, 04 Feb 2023 17:14:59.737597000 UTC +00:00>]
```
