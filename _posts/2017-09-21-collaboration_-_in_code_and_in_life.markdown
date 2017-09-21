---
layout: post
title:  "Collaboration - In Code and In Life!"
date:   2017-09-20 20:07:43 -0400
---


Why can't we all get along?  I've heard that for a very long time--even more so in today's politics.

So, why can't today's politicians get along? -- they haven't learned the art of Ruby's "Collaboration"!!

The "belongs to"/"has many" concept is a great bridge when you need to have Classes interact with each other.  They must work together or your code falls apart when you want to identify those real world examples.  

For example, while we've spent many hours working on lessons and labs with the Class Artist and Class Song since an Artist can have many songs, while a song belongs to only 1 Artist (unless of course, you collaborate amongst various Artists for that 1 great hit song, but that's a story for another day), there are other real world, everyday examples.

A real world example is utilities have many customers, while each customer belongs to a utility  (in these green times, a customer can choose a different utility in which they receive their energy than the utility which delivers that energy with the distribution and transmission lines) while a utility has many customers.

To get things rolling, I've created the Utility class.

```
class Utility
  attr_accessor :name, :customer
  @@all = []

  def initialize(name)
    @name = name
    @customers = []
  end

  def self.all
     @@all
  end

  def add_customer (customer)
    @customers << customer
  end

  def save
  self.class.all << self
  end

  def self.create
    utility = self.new(name)
    utility.save
    utility
  end

  def self.find_by_name(name)
    self.all.detect {|utility| utility.name == name}
  end

  def self.create_by_name(name)
    utility = self.create
    utility.name = name
    utility
  end

  def self.find_or_create_by_name(name)
    self.find_by_name(name) ? self.find_by_name(name) : self.create_by_name(name)
  end

  def print_customers
    customers.each {|customer| puts customer.name}
  end
end

```

I can build an array of customers, add to that array, find a utility by name, create a new utility if one doesn't exist, and print a list of customers.
But, we need to associate a customer with a utility --that's right, we need a Customer class to collaborate with the Utility class. 


```
class Customer
  attr_accessor :name, :utility

    def initialize (name)
      @name = name
    end

    def self.new_by_filename (file_name)
      customer = file_name.split(" - ")[1]
      new_customer = self.new(customer)
      utility = file_name.split(" - ")[0]
      new_customer.utility_name = customer
      new_customer
    end

    def utility_name= (name)
      self.utility = Utility.find_or_create_by_name(name)
      utility.add_customer(self)
    end
end

```

The collaboration within the Customer Class is using the #utility_name= method, which is then used in the #self.new_by_filename method to associate a new_customer with a utility with the code: new_customer.utility_name = customer.


However, there is another big piece of the puzzle, where do we get the data for all of the utilities and customers?  We need to pass a data file that lists Utilities across the country with their respective customers into an Importer Class to we can parse the data.  This file is in the .csv format and we will retrieve it from a specific directory path. This file will be parsed to eliminate the .csv and to retrieve the customer and the utility from the file and then import this data to use in the Customer Class above.

```
class CSVImporter
    attr_accessor :path

  def initialize(path)
    @path = path
  end

  def files
    @files = Dir.glob("#{path}/*.csv")
    @files.collect {|customer| customer.gsub("#{path}/","")}
  end

  def import
      files.each {|file_name|
        Customer.new_by_filename(file_name)}
  end
end
```

The CSVImported Class collaborates with the Customer Class by using the #import method-- as you can see the code uses files.each {|file_name|Customer.new_by_filename(file_name)} where "Customer" is the Class name.  Remember, we are in the CSVImporter Class using the class name Customer.

So at the end of the day, (1) the Utility Class uses:   :customer (setter/getter method) to reach out to the customer Class, (2) the Customer Class uses:  the utility_name= method to bridge the customer/utlity relationship, and (3) the CSVImporter Class uses:  the #import method to get the customer name to the Customer Class.

All of these Classes collaborate together, now it's time for the politicians of the world to do the same.











