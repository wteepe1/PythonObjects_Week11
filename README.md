# Object-Oriented Programming (OOP)

## Additional Resources

[Python Class Documentation](https://docs.python.org/3/tutorial/classes.html)

## Jupyter Notebook

The code associated with these notes can be executed in the `CreatingPythonObjects.ipynb` notebook in this repository.

## What is an Object

There are different styles for writing programs, but one very useful style is known as object-oriented programming (or OOP, for short). OOP is used in many different languages, including Python. OOP focuses on the idea of objects as the nouns of a programming language. We've already been using many of the standard object types in Python, such as integers, floats, strings, lists, tuples, and dictionaries. Nouns (objects) can be modified or used by verbs (functions and methods). Methods can be thought of as the behaviors of an object. They are actions that are intrinsic to the object itself. Functions are actions extrinsic to an object.

As our programming tasks become more complicated or specialized, we will often find that the standard object types in Python do not fully meet our needs. Instead, we can invent and define our own types of objects (like we did with custom functions last week). Custom objects will have their own properties (variables) and behaviors (methods). When we define new objects, we also define what these properties and behaviors can be.

When talking about creating our own objects, we will need to distinguish between object classes and object instances. Think of an object class as a generic description of a type of object. For instance, "book" could be an object class. We know that books have titles and authors, but we don't need to think of a specific title or author to think about the idea of a book. However, an instance of an object _will_ have specific properties. For instance, our textbook is a particular instance of a book with the title, "Practical Computing for Biologists", and the authors, "Haddock and Dunn".

The requirements of a custom type of object are entirely dependent on the goals of your code. What information does this object need in order to define itself? What actions will the object perform? A very important skill for any programmer is to be able to think about a programming goal, and break that goal down into the objects and functions that will be needed to accomplish it.

## Creating Custom Objects in Python (an example)

Let's say we're doing a study where we collect different organisms and want to store information about them. In this case, we'll define a new `organism` class. Note how the structure of our class definition is similar to how we defined new functions, but we use the keyword `class`. Class definitions also have docstrings, like functions.

After we define the class name and describe the class with a docstring, we can start to define the properties (variables) that instances of this class will have. In the case of organisms that we might collect, they could be assigned a unique `id` number, they will be assigned to a `species`, their location will be recorded with a `latitude` and `longitude`, and their characteristics will be measured and stored in `length` and `color` variables. Note that we've assigned default values to each of these characteristics. If an instance of `organism` is created without assigning values to these properties, these are the values they will have.

```
class organism:
	"""Class to hold information about organisms I collect."""

	id = ""
	species = "" # Latin name
	latitude = 0.0
	longitude = 0.0
	length = 0.0 # units are mm
	color = ""
```

Now, as we collect organisms, we can create new instances of this class and assign the appropriate properties to the different instances. To create a new instance of the `organism` class, we can call the name of the class like a function

`ind_1 = organism()`

Now, we can ask about the current values of `ind_1`'s properties. For example, if we look at its `latitude`, we should see the default value of 0.0.

`print(ind_1.latitude)`

However, if we try to look up a property that doesn't exist (like height), we should get an error.

`print(ind_1.height)`

Note how we're accessing the properties of an instance by using the dot operator - `.`. We type the name of the instance, followed by `.`, followed by the name of the property.

Assigning values to the properties of our new object is just as easy as assigning values to any other variable.

```
# Assign values to the properties of ind_1
ind_1.id = "LSUMNS 45884"
ind_1.species = "Hyla_cinerea"
ind_1.latitude = 30.418419
ind_1.longitude = -91.161132
ind_1.length = 40.0
ind_1.color = "green"

# Check the properties we've set
print(ind_1.id)
print(ind_1.species)
print(ind_1.latitude)
print(ind_1.longitude)
print(ind_1.length)
print(ind_1.color)
```

```
Practice Exercise 1

(1) Find the closest bookshelf or think of your 3 favorite books and look them up online.

(2) Create a custom book object class that has these properties: title, author(s), publisher, year.

(3) Create 3 instances of the book class and assign values

(4) Create a list called `bookshelf = []` and add your books to this list.
```


## Object Methods

In addition to properties, objects can also have associated functions (i.e., methods) that define the possible behaviors of those objects. For example, let's say that we wanted an easy way to print out the location information about our organism. We could add a `printLatLon()` method to our class definition

```
class organism:
    """Class to hold information about organisms I collect."""

    id = ""
    species = "" # Latin name
    latitude = 0.0
    longitude = 0.0
    length = 0.0 # units are mm
    color = ""

    def printLatLon(self):
        """This method prints the latitude and longitude."""

        print("(%f,%f)" % (self.latitude,self.longitude))
```

To show how this can be used

```
ind_1 = organism()
ind_1.latitude = 30.418419
ind_1.longitude = -91.161132

ind_1.printLatLon()
```

Note that the `self` variable is a special reference to that instance of the object. So, to reference a property of the object from inside one of its methods, you'll need to start that variable reference with `self.<VARIABLE>`. Also, `self` _must_ be the first argument to all class methods.

We could also create a more comprehensive method that printed out a nicely formatted record of all information about an organism

```
class organism:
    """Class to hold information about organisms I collect."""

    id = ""
    species = "" # Latin name
    latitude = 0.0
    longitude = 0.0
    length = 0.0 # units are mm
    color = ""

    def printLatLon(self):
        """This function prints the latitude and longitude."""

        print("(%f,%f)" % (self.latitude,self.longitude))

    def printSummary(self):
        """This methods prints all info about organism."""

        print("Organism %s is an individual of the species %s. It was collected at (%f,%f). It is %f mm long and is %s." % (self.id,self.species,self.latitude,self.longitude,self.length,self.color))

# Let's test our new `.printSummary()` method
ind_1 = organism()
ind_1.id = "LSUMNS 45884"
ind_1.species = "Hyla_cinerea"
ind_1.latitude = 30.418419
ind_1.longitude = -91.161132
ind_1.length = 40.0
ind_1.codlor = "green"

ind_1.printSummary()
```



## Object Constructors

To set variables for each instance of an object in an organized way, objects have a special method known as a constructor. The purpose of the constructor is to provide a cohesive way to create new instances of a given class and set all variables appropriately as soon as the instance is created. The constructor always has a specific name with a special meaning: `__init__`.

Usually, if you want to define the values of variables for that instance, they are passed as arguments to the constructor. So, to define a constructor for our organism class, we could do the following:

```
class organism():
    """Class to hold information about organisms I collect."""

    def __init__(self,id,sp,lat,lon,length,col):
        self.id = id
        self.species = sp
        self.latitude = lat
        self.longitude = lon
        self.length = length
        self.color = col
```

Now, to create `ind_1` with the same properties as above, we could do this: 

```
# I'm passing each argument on its own line, to make it easy to read

ind_1 = organism("LSUMNS 45884",
                 "Hyla_cinerea",
                 30.418419,
                 -91.161132,
                 40,
                 "green")
```

You can also create default values for all the variables, which are included in the list of arguments to the constructor

```
class organism:
    """Class to hold information about organisms I collect."""

    def __init__(self,id="1",sp="genus_species",lat=0.0,lon=0.0,length=0.0,col="black"):
        self.id = id
        self.species = sp
        self.latitude = lat
        self.longitude = lon
        self.length = length
        self.color = col
```

You can then create new individuals without passing any arguments to the constructor and the variables will take the default values:

```
ind_1 = organism() 
ind_1.id
```

You can also pass just those variables you want to define, with the others taking default values:

```
ind_1 = organism(col="green")
ind_1.id
ind_1.color
```

## Working With Objects

Now that we've defined our custom class, we can work with instances of this class (objects) just like we would any other variable. For instance, we can create a list of organisms: 

```
myOrganisms = []

myOrganisms.append(organism(id="45",sp="Anolis_carolinensis"))
myOrganisms.append(organism(id="23",sp="Hemidactylus_turcicus",col="grey"))

print(myOrganisms)

for org in myOrganisms:
    print(org.id)
    print(org.color)
```

We can also write custom functions that use our new objects

```
def sameSpecies(org1,org2):
    if org1.species == org2.species:
        return True
    else:
        return False

sameSpecies(myOrganisms[0],myOrganisms[1])

newOrg = organism(id=72,sp="Anolis_carolinensis")

sameSpecies(myOrganisms[0],newOrg)
```

```
Practice Exercise 2

(1) Add a describe() method to the book class you created above that prints out a 
    sentence or two summarizing information about a book.

(2) Create a function to check whether two books are the same. To be the same, all information 
    (title, authors, etc.) about the books should be the same.
```
