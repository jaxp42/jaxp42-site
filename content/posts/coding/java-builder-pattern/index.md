---
title: "Java Builder Pattern"
date: 2023-04-04T23:25:24+01:00
draft: false
tags: ["Java", "OOP", "Best practices", "Efective Java"]
summary: "The builder pattern and the covariant return typing in Java"
---

In this post we are going to see one of the most popular and useful patterns to instantiate objects. The Builder pattern.


## The Java Builder Pattern

When we have a class with too many attributes we face a problem when we need to define the different constructors for all the combinations. A first approach could be using the **telescoping constructor pattern**. This approach consists in defining a constructor for all the different combinations from the constructor with fewer parameters to the one with the most. These solutions have some disadvantages like the number of constructors that we must do and the difficulty to read constructors with a huge amount of parameters.

Another solution is using the **JavaBeans pattern**. This solution consists in using just one constructor without parameters and then using the setters to pass all the information to the object. This has a very serious inconvenience. Using the object while it is being built with the setter could have unexpected behaviour.

So, for solving this problem the best option is to use the **Builder pattern**. With the builder pattern, we have the best of the two previous patterns. **We have the security of the telescoping pattern and the readability of a Java bean.**

How does it work? Well, instead of creating an object directly we will create a Builder object (this Builder class is usually an inner class of the object we are trying to instantiate). This object will have methods that are some sort of set methods which we will use to set all the properties we want. Each of **these methods will return the same Builder object that will allow us to chain all the invocations**. In the end, we have a build method that will create and return the object we want to instantiate with all the data we passed to the builder object.


## Builder example

Let’s suppose we want to create an Order object. This order has an id, an address to deliver, a list of products and an optional comment from the client. This will be the resulting code:


    public class Order {
        private final int id;
        private final String address, comment;
        private final List<String> products;

        public Order(Builder builder){
            this.id = builder.id;
            this.address = builder.address;
            this.comment = builder.comment;
            this.products = builder.products;
        }

        public static class Builder{
            //required attributes
            private final int id;
            private final String address;

            //optional attributes
            private List<String> products = new ArrayList<>();
            private String comment = "";

            public Builder(int id, String address){
                this.id = id;
                this.address = address;
            }

            public Builder addProduct(String product){
                products.add(product);
                return this;
            }

            public Builder comment(String comment){
                this.comment = comment;
                return this;
            }

            public Order build(){
                return new Order(this);
            }
        }
    }

As you can see the Builder “set” methods assign the value to the attribute and then return the same Builder object. And the build method calls the Order constructor and passes the builder as a parameter returning the resulting Order object. An example of the use of this pattern on the client side:

    public class Client {
        public static void main(String[] args){
            Order order = new Order.Builder(1, "My address")
                    .addProduct("Graphic card")
                    .addProduct("Mother board")
                    .addProduct("SSD drive")
                    .comment("Phone before deliver")
                    .build();
        }
    }


This is a quite basic implementation of the Builder pattern but it's enough to understand its purpose and usage. In summary, the Builder pattern is a good option when constructors would have more parameters than it is comfortable to handle.

