---
layout: post
title:      "Creating a Coffee Shop Chatbot"
date:       2020-11-28 23:30:53 +0000
permalink:  creating_a_coffee_shop_chatbot
---


![chatbots](https://i.imgur.com/LcI1UMq.png?1)

Chatbots have become a normal part of our daily lives. Whether it's getting a greeting when we open up a car sales website or even saying "Hey Siri/Alexa/Google" in our homes, interacting with chatbots has become a normalized experience. Historically, the idea of chatbots and being able to create such a system can be traced back to Alan Turing and his work with AI to decode messages in the war. Still the Turing test is a standard when developing AI chatbots to be able to test if you are interacting with a computer or human. 

There are different types of chatbots. We interact with chatbots with both text and voice. Typically the chatbot greets a user and then asks how it can help the user. More complex chatbots can continue a conversation and is able to parse the users' responses to be able to help the user. These are assistant chatbots.

Some basic chatbots simply follow certain rules and give responses based on how well it is able to parse the user's input.

Chatbots have become increasingly helpful, not only with making a user experience more streamlined - but now in the new era of wanting to keep socially distant and decrease interpersonal interactions, having chatbots take restaurant orders or coffee shop orders can help both customers and workers to not be exposed to Covid19.

![coffeeshop](https://i.imgur.com/w0XV6D3.jpg?1)

With every chatbot, the first thing to initialize is to create a greeting. For all of these interactions we are going to use the input() function to be able to prompt a user's response and store it.

```
# Greeting
def coffee_bot():
  print("Welcome to the cafe!")
```

Next we are a coffee shop so the very next thing to ask would be what drink would they want?

```
def get_drink_type():
  res = input("What type of drink would you like? \n[a] Brewed Coffee \n[b] Mocha \n[c] Latte \n>")
  if res == 'a':
    return 'brewed coffee'
  elif res == 'b':
    return 'mocha'
  elif res == 'c':
    return 'latte'

```
This will take in "a", "b", or "c" but if our user input's something else, we should probably have a way to prompt them for one of these responses. Along with that, we should lowercase their response with the .lower() method to make sure our bot can receive it.

```
# Message for an input that doesn't match our selection options.
def print_message():
  print ("I'm sorry, I did not understand your selection. Please enter the corresponding letter for your response.")
	
# Adding that to our function and prompting them to answer again if the input isn't recognized	
def get_drink_type():
  res = input("What type of drink would you like? \n[a] Brewed Coffee \n[b] Mocha \n[c] Latte \n>").lower()
  if res == 'a':
    return 'brewed coffee'
  elif res == 'b':
    return 'mocha'
  elif res == 'c':
    return order_latte()
  else:
    print_message()
    return get_drink_type()
```

Of course nowadays, lattes are served with all sorts of alternative milks so wanting to get that correct is important.
![milks](https://i.imgur.com/5KbrYc5.jpg?1)

```
def order_latte():
  milk = input("And what kind of milk for your latte? \n[a] 2% milk \n[b] Non-fat milk \n[c] Soy milk \n[d] Almond milk \n[e] Oatmilk \n[f] Hemp milk>").lower()
  if milk == 'a':
    return 'latte'
  elif milk == 'b':
    return 'non-fat latte'
  elif milk == 'c':
    return 'soy latte'
	elif milk == 'd':
	  return 'almond latte'
	elif milk == 'e':
	  return 'oat latte'
	elif milk == 'f':
	  return 'hemp latte'
  else:
    print_message()
    return order_latte()
```

So we can add the order_latte into our drink type function:
```
def get_drink_type():
  res = input("What type of drink would you like? \n[a] Brewed Coffee \n[b] Mocha \n[c] Latte \n>").lower()
  if res == 'a':
    return 'brewed coffee'
  elif res == 'b':
    return 'mocha'
  elif res == 'c':
    return order_latte()
  else:
    print_message()
    return get_drink_type()
```

Next, we will want to know what size they will want to order, so we will use a similar fashion as before:
```
def get_size():
  res = input('What size drink can I get for you? \n[a] Small \n[b] Medium \n[c] Large \n> ').lower()
  if res == 'a':
    return 'small'
  elif res == 'b':
    return 'medium'
  elif res == 'c':
    return 'large'
  else:
    print_message()
    return get_size()
```

Notice we use our message anytime we need to have a user change their input to match what our bot is anticipating.

Finally, we can ask about what type of cup they are using:
```
def extra_options():
  res = input('Do you want a mug for here, a cup to-go, or a refill of your own cup? \n[a] I\'ll have it here in a mug. \n[b] I'll take a plastic cup to go! \n[c] I brought my own! > ').lower()
 
 if res == 'a':
   print('Awesome! Your order will be ready in a mug soon!')
 elif res == 'b':
    print('Okay, no problem! We\'ll get you a plastic cup.')
  elif res == 'c':
    print('Great! We\'ll fill up your reusable cup.')
  else:
    print_message()
    return extra_options()
```

Now we can add all of these to our coffee_bot and run the program:
```
def coffee_bot():
  print("Welcome to the cafe!")
  size = get_size()
  drink_type = get_drink_type()
  print("Alright, that's a {} {}.".format(size, drink_type))

  extra_options()
  
  name = input("Can I get your name please?")
  print("Thanks, {}! Your drink will be ready shortly".format(name))

coffee_bot()
```

To run the program, make sure you have an instance of your coffee_bot and then open the python script in your favorite code editor and see how it runs. For the full bot, you can use this:
```
def print_message():
  print ("I'm sorry, I did not understand your selection. Please enter the corresponding letter for your response.")

def extra_options():
  res = input('Do you want a mug for here, a cup to-go, or a refill of your own cup? \n[a] I\'ll have it here in a mug. \n[b] I'll take a plastic cup to go! \n[c] I brought my own! > ').lower()
 
 if res == 'a':
   print('Awesome! Your order will be ready in a mug soon!')
 elif res == 'b':
    print('Okay, no problem! We\'ll get you a plastic cup.')
  elif res == 'c':
    print('Great! We\'ll fill up your reusable cup.')
  else:
    print_message()
    return extra_options()
              
def get_drink_type():
  res = input("What type of drink would you like? \n[a] Brewed Coffee \n[b] Mocha \n[c] Latte \n>").lower()
  if res == 'a':
    return 'brewed coffee'
  elif res == 'b':
    return 'mocha'
  elif res == 'c':
    return order_latte()
  else:
    print_message()
    return get_drink_type()
  
def order_latte():
  milk = input("And what kind of milk for your latte? \n[a] 2% milk \n[b] Non-fat milk \n[c] Soy milk \n[d] Almond milk \n[e] Oatmilk \n[f] Hemp milk>").lower()
  if milk == 'a':
    return 'latte'
  elif milk == 'b':
    return 'non-fat latte'
  elif milk == 'c':
    return 'soy latte'
	elif milk == 'd':
	  return 'almond latte'
	elif milk == 'e':
	  return 'oat latte'
	elif milk == 'f':
	  return 'hemp latte'
  else:
    print_message()
    return order_latte()

def extra_options():
  res = input('Would you like a plastic cup or did you bring your own reusable cup? \n[a] I\'ll need a cup. \n[b] Brought my own! \n> ')
  
  if res == 'a':
    print('Okay, no problem! We\'ll get you a plastic cup.')
  elif res == 'b':
    print('Great! We\'ll fill up your reusable cup.')
  else:
    print_message()
    return extra_options()


def get_drink_type():
  res = input("What type of drink would you like? \n[a] Brewed Coffee \n[b] Mocha \n[c] Latte \n>").lower()
  if res == 'a':
    return 'brewed coffee'
  elif res == 'b':
    return 'mocha'
  elif res == 'c':
    return order_latte()
  else:
    print_message()
    return get_drink_type()
  
def get_size():
  res = input('What size drink can I get for you? \n[a] Small \n[b] Medium \n[c] Large \n> ').lower()
  if res == 'a':
    return 'small'
  elif res == 'b':
    return 'medium'
  elif res == 'c':
    return 'large'
  else:
    print_message()
    return get_size()

def coffee_bot():
  print("Welcome to the cafe!")
  size = get_size()
  drink_type = get_drink_type()
  print("Alright, that's a {} {}.".format(size, drink_type))

  extra_options()
  
  name = input("Can I get your name please?")
  print("Thanks, {}! Your drink will be ready shortly".format(name))
 
coffee_bot()

```


