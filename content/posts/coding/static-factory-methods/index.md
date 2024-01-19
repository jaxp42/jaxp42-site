---
title: "Static Factory Methods"
date: 2023-02-26T16:04:43+01:00
draft: false
tags: ["Java", "OOP", "Effective Java", "Best practices"]
summary: "Using static factory methods instead of constructors is a good practice. When and how to use them?"
---

## Static factory methods

In this post, we are learning an alternative way to instantiate objects. Instead of using the class constructor, we can use static methods to create an object. This option has some advantages and disadvantages that we will see next.

## Naming

The first, and most obvious, advangtage, is the possibility of naming the methods in a clearer way. Instead of havind different constructors with the name of the class we can set a name for the method that describes what is being builded. E.g.

Having the following class:

    public class Example {
        String attribute1, attribute2;
        
        public Example(){
            this.attribute1 = "default 1";
            this.attribute2 = "default 2";
        }
    }

We can see that we are using the default constructor to set the attributes to a default value. Because of the method signature, we can not have a way to create an empty object or let's suppose we want to give the constructor one of the attributes and set the other one to default. Then we would need a constructor to pass a String and set the other property to the default value, but again, because of the signature, we can't do that. The other way is inserting logic into the constructor to check if we passed one attribute or another, and that's never a good idea. Instead we could use static factory methods like this:

    public class Example {
        String attribute1, attribute2;

        private Example(){}

        private Example(String attribute1, String attribute2){
            this.attribute1 = attribute1;
            this.attribute2 = attribute2;
        }

        public static Example createEmptyExample(){
            return new Example();
        }

        public static Example createExampleWithDefaultValues(){
            return new Example("default 1", "default 2");
        }

        public static Example createExampleWithDefaultAttribute2(String attribute1){
            return new Example(attribute1, "default 2");
        }

        public static Example createExampleWithDefaultAttribute1(String attribute2){
            return new Example("default 1", attribute2);
        }
    }

We are solving the problem and the class is much more clear to use. But **this has a problem too**. The client has to know the method names. This must be well documented but there is too a naming convention for the static factory methods according to the use of the method. You can see them [here](https://jorgenringen.github.io/2018/01/factory_method_naming_conventions/)


## Immutability

As we control the creation of the object with the static factory we can check if already exists an instance of the object we are trying to create and return it instead of creating a new one. This way we avoid to create duplicated objects. E.g.

Following with the previous class (**Example**). We can check if we already have a default **Example** object created and return it to avoid create more equal objects.

    private static Example defaultExample = null;


    public static Example createExampleWithDefaultValues(){
        if(defaultExample == null){
            defaultExample = new Example("default 1", "default 2");
        }

        return  defaultExample;
    }

In case of objects expensive to create this can improve the performance if they are created very often.


## Returning object flexibility

This is the point I find more interesting. Combining the Java Interfaces and the static factory methods is possible to create an API hiding its implementation from the user. In this case, the public static methods must be in the interface (this is possible since Java 8) and the implementation classes must implement this interface and be hidden from the user but visible to the interface. We can manage this with the appropriate package permissions and structure.

With this approach the user only has to work with the interface and doesn't care about the class implementation that is returning. This makes the api logic completely independent from the user. We will be able to modify or even change the returning types of the classes without affecting the client code. Let's see the next example.

Having the next interface **Person:**

    public interface Person {

        static Person of(String name, String surname, int age){
            if(age < 18){
                return Kid.of(name, surname, age);
            }
            else{
                return Adult.of(name, surname, age);
            }
        }

        void greet();
    }

And its implementation classes

**Adult:**

    public class Adult implements Person{
        String name, surname;
        int age;

        private Adult(String name, String surname, int age){
            this.name = name;
            this.surname = surname;
            this.age = age;
        }

        protected static Adult of(String name, String surname, int age){
            Adult adult = new Adult(name, surname, age);
            return adult;
        }

        @Override
        public void greet() {
            System.out.println("Hello, My name is " + name + " " + surname);
        }
    }

**Kid:**

    public class Kid implements Person {
        private String name, surname;
        private int age;

        private Kid(String name, String surname, int age){
            this.name = name;
            this.surname = surname;
            this.age = age;
        }

        protected static Kid of(String name, String surname, int age){
            Kid kid = new Kid(name, surname, age);
            return kid;
        }

        @Override
        public void greet() {
            System.out.println("Hi, I'm " + name);
        }
    }

The cliend could work with the api as follows.

    public class Client {
        public static void main(String[] args){
            Person adult = Person.of("Marco", "Ulpio Trajano", 30);
            Person kid = Person.of("Tito", "Domiciano", 15);
            Person baby = Person.of("Domicia", "Longina", 1);

            adult.greet();
            kid.greet();
            baby.greet();
        }
    }

This will have the next output:

    Hello, My name is Marco Ulpio Trajano
    Hi, I'm Tito
    Hi, I'm Domicia

Now let's suppose the Api is not finished yet. And we add a new implementation class and modify the creation logic. So we add the **Baby** class:

    public class Baby implements Person{
        private String name, surname;
        private int age;

        private Baby(String name, String surname, int age){
            this.name = name;
            this.surname = surname;
            this.age = age;
        }

        protected static Baby of(String name, String surname, int age){
            Baby baby = new Baby(name, surname, age);
            return baby;
        }

        @Override
        public void greet(){
            System.out.println("Buaaaaahhhhh");
        }
    }

And modify the **Person** static factory method:

    static Person of(String name, String surname, int age){
        if(age <= 1){
            return Baby.of(name, surname, age);
        }
        else if(age < 18){
            return Kid.of(name, surname, age);
        }
        else{
            return Adult.of(name, surname, age);
        }
    }

The client doesn't change anything and doesn't know what have change. The output with the same code is now:

    Hello, My name is Marco Ulpio Trajano
    Hi, I'm Tito
    Buaaaaahhhhh

This is the basic usage of the static factory methods. To this last example we could have added a method to check if already exists a person with the same name and age and returned it instead of creating a new one. You can see the full code [in my github](https://github.com/jaxp42/EffectiveJava/tree/main/src/creatingObjects/staticFactoryConstruction)