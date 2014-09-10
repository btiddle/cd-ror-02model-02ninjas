| | |
|:---------------|:-------------------|
|**Domain** | Coding Dojo (cd)|
|**Areas of Study** | Ruby on Rails (ror)<br> Understanding Rails - Models (02model)|
|**Assignment**| Dojo Ninjas (02ninjas)|
|**Repository** | cd-ror-02model-02ninjas |
<br>
<pre>

===============
#1: Start a new project (the name of the project should be 'dojo_ninjas')
===============

rails new dojo-ninjas
cd dojo-ninjas

edit> Gemfile
  gem 'bcrypt', '~> 3.1.7'    # Using bcrypt (3.1.7)
  gem 'foundation-rails'      # Using foundation-rails (5.4.0.0)
  gem 'hirb'                  # Using hirb (0.7.2)
bundle install

===============
#2: Create appropriate tables/models using "rails generate model", "rake db:create", and "rake db:migrate"
===============

rails generate model Dojo name:string city:string state:string
rails generate model Ninja first_name:string last_name:string dojo:references

edit> dojo.rb
class Dojo < ActiveRecord::Base
    has_many :ninjas
end

rake db:create
rake db:migrate

===============
#3: Using ruby console, create 3 dojos (insert some blank entries just to make
    sure it's allowing you to insert empty entries.
===============

cd dojo-ninjas
rail c
Hirb.enable() 

Dojo.create()
Dojo.all
Ninja.create()
Ninja.all

Dojo.find(1).update(name: "CodingDojo Silicon Valley", city: "Mountain View", state: "CA")
Dojo.create(name: "CodingDojo Seattle", city: "Seattle", state: "WA")
Dojo.create(name: "CodingDojo New York", city: "New York", state: "NY")
Dojo.all

+----+---------------------------+---------------+-------+-------------------------+-------------------------+
| id | name                      | city          | state | created_at              | updated_at              |
+----+---------------------------+---------------+-------+-------------------------+-------------------------+
| 1  | CodingDojo Silicon Valley | Mountain View | CA    | 2014-06-04 15:18:31 UTC | 2014-06-04 15:21:59 UTC |
| 2  | CodingDojo Seattle        | Seattle       | WA    | 2014-06-04 15:23:47 UTC | 2014-06-04 15:23:47 UTC |
| 3  | CodingDojo New York       | New York      | NY    | 2014-06-04 15:26:07 UTC | 2014-06-04 15:26:07 UTC |
+----+---------------------------+---------------+-------+-------------------------+-------------------------+

===============
#4: Change your models so that it does the following validations
    - For the dojo, require the presence of the name, city, and state; also
      require the state to be two characters long
    - For the ninja, require the presence of the first name and last name
===============

edit> dojo.rb
  - add: validates :name, :city, :state, presence: true
  - add: validates :state, length: { is: 2 }, numericality: false

edit> ninja.rb
  - add: validates :first_name, :last_name, presence: true

reload!
dojoA = Dojo.new
dojoA.valid?
dojoA.errors.full_messages

=> ["Name can't be blank", "City can't be blank", "State can't be blank", "State is the wrong length (should be 2 characters)"]

ninjaA = Ninja.new
ninjaA.valid?
ninjaA.errors.full_messages

=> ["First name can't be blank", "Last name can't be blank"]

===============
#5: Make sure that your ninja model belongs to the dojo (specify this in the
    model for both the dojo and the ninja)
===============

This was done in step #2 when models were created.

===============
#6.1: Delete the three dojos you created (e.g. Dojo.find(1).destroy; also look up destroy_all method)
===============

Dojo.all
Dojo.destroy_all()
Dojo.all

=> #<ActiveRecord::Relation []>

===============
#6.2: Create 3 additional dojos by using Dojo.new().
===============

dojo01 = Dojo.new(name: "CodingDojo Silicon Valley", city: "Mountain View", state: "CA")
dojo01.save

dojo02 = Dojo.new(name: "CodingDojo Seattle", city: "Seattle", state: "WA")
dojo02.save

dojo03 = Dojo.new(name: "CodingDojo New York", city: "New York", state: "NY")
dojo03.save

Dojo.all

+----+---------------------------+---------------+-------+-------------------------+-------------------------+
| id | name                      | city          | state | created_at              | updated_at              |
+----+---------------------------+---------------+-------+-------------------------+-------------------------+
| 4  | CodingDojo Silicon Valley | Mountain View | CA    | 2014-06-04 16:02:26 UTC | 2014-06-04 16:02:26 UTC |
| 5  | CodingDojo Seattle        | Seattle       | WA    | 2014-06-04 16:02:43 UTC | 2014-06-04 16:02:43 UTC |
| 6  | CodingDojo New York       | New York      | NY    | 2014-06-04 16:03:03 UTC | 2014-06-04 16:03:03 UTC |
+----+---------------------------+---------------+-------+-------------------------+-------------------------+

===============
#6.3: Try to create a few more dojos but without specifying some of the required
      fields. Make sure that the validation works and generates the errors.
===============

dojo04 = Dojo.new
dojo04.valid?
dojo04.errors.full_messages
=> ["Name can't be blank", "City can't be blank", "State can't be blank", "State is the wrong length (should be 2 characters)"]

dojo05 = Dojo.new(name: "CodingDojo Austin")
dojo05.valid?
dojo05.errors.full_messages
=> ["City can't be blank", "State can't be blank", "State is the wrong length (should be 2 characters)"]

dojo06 = Dojo.new(name: "CodingDojo Austin", city: "Austin")
dojo06.valid?
dojo06.errors.full_messages
=> ["State can't be blank", "State is the wrong length (should be 2 characters)"]

===============
#6.4: Create 3 ninjas that belong to the first dojo you created
===============

Ninja.create(first_name: "Alice", last_name: "Baker", dojo_id: 4)
Ninja.create(first_name: "Cindy", last_name: "Davis", dojo_id: 4)
Ninja.create(first_name: "Elaine", last_name: "Franks", dojo_id: 4)

Ninja.all
+----+------------+-----------+---------+-------------------------+-------------------------+
| id | first_name | last_name | dojo_id | created_at              | updated_at              |
+----+------------+-----------+---------+-------------------------+-------------------------+
| 1  |            |           |         | 2014-06-04 15:18:53 UTC | 2014-06-04 15:18:53 UTC |
| 2  | Alice      | Baker     | 4       | 2014-06-04 16:14:51 UTC | 2014-06-04 16:14:51 UTC |
| 3  | Cindy      | Davis     | 4       | 2014-06-04 16:15:39 UTC | 2014-06-04 16:15:39 UTC |
| 4  | Elaine     | Franks    | 4       | 2014-06-04 16:16:44 UTC | 2014-06-04 16:16:44 UTC |
+----+------------+-----------+---------+-------------------------+-------------------------+

===============
#6.5: Create 3 more ninjas and have them belong to the second dojo you created
===============

Ninja.create(first_name: "Glinda", last_name: "Haris", dojo_id: 5)
Ninja.create(first_name: "India", last_name: "Jones", dojo_id: 5)
Ninja.create(first_name: "India", last_name: "Jones", dojo_id: 5)
Ninja.find(7).update(first_name: "Kate", last_name: "Lewis", dojo_id: 5)

Ninja.all
+----+------------+-----------+---------+-------------------------+-------------------------+
| id | first_name | last_name | dojo_id | created_at              | updated_at              |
+----+------------+-----------+---------+-------------------------+-------------------------+
| 1  |            |           |         | 2014-06-04 15:18:53 UTC | 2014-06-04 15:18:53 UTC |
| 2  | Alice      | Baker     | 4       | 2014-06-04 16:14:51 UTC | 2014-06-04 16:14:51 UTC |
| 3  | Cindy      | Davis     | 4       | 2014-06-04 16:15:39 UTC | 2014-06-04 16:15:39 UTC |
| 4  | Elaine     | Franks    | 4       | 2014-06-04 16:16:44 UTC | 2014-06-04 16:16:44 UTC |
| 5  | Glinda     | Haris     | 5       | 2014-06-04 16:20:49 UTC | 2014-06-04 16:20:49 UTC |
| 6  | India      | Jones     | 5       | 2014-06-04 16:20:57 UTC | 2014-06-04 16:20:57 UTC |
| 7  | Kate       | Lewis     | 5       | 2014-06-04 16:20:59 UTC | 2014-06-04 16:22:28 UTC |
+----+------------+-----------+---------+-------------------------+-------------------------+

===============
#6.6: Create 3 more ninjas and have them belong to the third dojo you created
===============

Ninja.create(first_name: "Mary", last_name: "Norway", dojo_id: 6)
Ninja.create(first_name: "Olivia", last_name: "Pain", dojo_id: 6)
Ninja.create(first_name: "Queen", last_name: "Rose", dojo_id: 6)

Ninja.all
+----+------------+-----------+---------+-------------------------+-------------------------+
| id | first_name | last_name | dojo_id | created_at              | updated_at              |
+----+------------+-----------+---------+-------------------------+-------------------------+
| 1  |            |           |         | 2014-06-04 15:18:53 UTC | 2014-06-04 15:18:53 UTC |
| 2  | Alice      | Baker     | 4       | 2014-06-04 16:14:51 UTC | 2014-06-04 16:14:51 UTC |
| 3  | Cindy      | Davis     | 4       | 2014-06-04 16:15:39 UTC | 2014-06-04 16:15:39 UTC |
| 4  | Elaine     | Franks    | 4       | 2014-06-04 16:16:44 UTC | 2014-06-04 16:16:44 UTC |
| 5  | Glinda     | Haris     | 5       | 2014-06-04 16:20:49 UTC | 2014-06-04 16:20:49 UTC |
| 6  | India      | Jones     | 5       | 2014-06-04 16:20:57 UTC | 2014-06-04 16:20:57 UTC |
| 7  | Kate       | Lewis     | 5       | 2014-06-04 16:20:59 UTC | 2014-06-04 16:22:28 UTC |
| 8  | Mary       | Norway    | 6       | 2014-06-04 16:28:14 UTC | 2014-06-04 16:28:14 UTC |
| 9  | Olivia     | Pain      | 6       | 2014-06-04 16:28:34 UTC | 2014-06-04 16:28:34 UTC |
| 10 | Queen      | Rose      | 6       | 2014-06-04 16:28:44 UTC | 2014-06-04 16:28:44 UTC |
+----+------------+-----------+---------+-------------------------+-------------------------+

===============
#6.7: Make sure you understand how to get all of the ninjas for any dojo (e.g. 
      get all the ninjas for the first dojo, second dojo, etc)
===============

Dojo.find(4).ninjas
or
Dojo.find_by(:name => "CodingDojo Silicon Valley").ninjas
+----+------------+-----------+---------+-------------------------+-------------------------+
| id | first_name | last_name | dojo_id | created_at              | updated_at              |
+----+------------+-----------+---------+-------------------------+-------------------------+
| 2  | Alice      | Baker     | 4       | 2014-06-04 16:14:51 UTC | 2014-06-04 16:14:51 UTC |
| 3  | Cindy      | Davis     | 4       | 2014-06-04 16:15:39 UTC | 2014-06-04 16:15:39 UTC |
| 4  | Elaine     | Franks    | 4       | 2014-06-04 16:16:44 UTC | 2014-06-04 16:16:44 UTC |
+----+------------+-----------+---------+-------------------------+-------------------------+

Dojo.find(5).ninjas
or
Dojo.find_by(:city => "Seattle").ninjas
+----+------------+-----------+---------+-------------------------+-------------------------+
| id | first_name | last_name | dojo_id | created_at              | updated_at              |
+----+------------+-----------+---------+-------------------------+-------------------------+
| 5  | Glinda     | Haris     | 5       | 2014-06-04 16:20:49 UTC | 2014-06-04 16:20:49 UTC |
| 6  | India      | Jones     | 5       | 2014-06-04 16:20:57 UTC | 2014-06-04 16:20:57 UTC |
| 7  | Kate       | Lewis     | 5       | 2014-06-04 16:20:59 UTC | 2014-06-04 16:22:28 UTC |
+----+------------+-----------+---------+-------------------------+-------------------------+

Dojo.find(6).ninjas
or
Dojo.find_by(:state => "NY").ninjas
+----+------------+-----------+---------+-------------------------+-------------------------+
| id | first_name | last_name | dojo_id | created_at              | updated_at              |
+----+------------+-----------+---------+-------------------------+-------------------------+
| 8  | Mary       | Norway    | 6       | 2014-06-04 16:28:14 UTC | 2014-06-04 16:28:14 UTC |
| 9  | Olivia     | Pain      | 6       | 2014-06-04 16:28:34 UTC | 2014-06-04 16:28:34 UTC |
| 10 | Queen      | Rose      | 6       | 2014-06-04 16:28:44 UTC | 2014-06-04 16:28:44 UTC |
+----+------------+-----------+---------+-------------------------+-------------------------+

===============
#6.8: Delete the second dojo. How could you adjust your model so that it
      automatically removes all of the ninjas associated with that dojo?
===============

edit> dojo.rb
  - edit: has_many :ninjas, dependent: :destroy       # Confirmed worked!
  or
  - edit: has_many :ninjas, :dependent => :destroy    # Confirmed worked also!

exit
rails c
or
reload!
Dojo.find(5).destroy

===============
#6.9: How would you only retrieve the first_names of the ninja that belongs to
      the second dojo and order the result by created_at DESC order?
===============

ninjas_temp = Dojo.find(6).ninjas
ninjas_temp.select("first_name").order("first_name DESC")
OR
Dojo.find(6).ninjas.select("first_name").order("first_name DESC")

+----+------------+-----------+---------+-------------------------+-------------------------+
| id | first_name | last_name | dojo_id | created_at              | updated_at              |
+----+------------+-----------+---------+-------------------------+-------------------------+
| 8  | Mary       | Norway    | 6       | 2014-08-25 13:17:43 UTC | 2014-08-25 13:17:43 UTC |
| 9  | Olivia     | Pain      | 6       | 2014-08-25 13:17:43 UTC | 2014-08-25 13:17:43 UTC |
| 10 | Queen      | Rose      | 6       | 2014-08-25 13:17:43 UTC | 2014-08-25 13:17:43 UTC |
+----+------------+-----------+---------+-------------------------+-------------------------+

+----+------------+
| id | first_name |
+----+------------+
|    | Queen      |
|    | Olivia     |
|    | Mary       |
+----+------------+

===============
The End!
===============

</pre>