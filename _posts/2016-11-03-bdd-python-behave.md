---
layout: post
title:  "Behavior-driven Development in Python using Behave"
date:   2016-11-03 09:37:10 +0600
categories: Python
permalink: /:slug
comments: true
thumbnail: /assets/bdd-behave.png
---
There are a few Behavior-driven Development (BDD) frameworks for Python. In this post, I kept a record of how I did it with behave.

I used [behave](http://pythonhosted.org/behave/) and [Radish](http://radish-bdd.io/), but [lettuce](http://lettuce.it/). In my opinion while giving Radish a whirl, it tried to become behave, but it didn't live up to the expectation. Although I was deceived by its nice homepage, only to figure that they are not only unmaintained, but also there is no decent example that actually runs. In addition, it was designed unnecessarily complex. For behave, there are bunch of [samples](http://jenisys.github.io/behave.example/).

![Behave's BDD results]({{ site.url }}/assets/bdd-behave.png)

## Gherkin
Like many BDD frameworks, behave is [Gherkin](https://en.wikipedia.org/wiki/Cucumber_(software)#Gherkin_.28Language.29) compatible, which in itself is a language that allows Business Analysts to write specifications in a Natural Language fashion. Take the following as an example. A scenario template can be written and a table of data can be tried out using that template onto business logic.      

```
Feature: Test pricing
	Customer provides details and the system returns a pricing. 
	To test that we have verified the following scenarios:
	
	Scenario Outline: Test pricing for source, destination, pax
		Given Customer specified from "<source>" to "<destination>" with "<pax>"
		When She queries for price
		Then She expects the pricing to be "<price>"

	Examples:
		| source	| destination	| pax	| price	|
		| Bergen	| Oslo		| 10	| 200	|
		| Oslo	        | Bergen	| 5	| 100	|
```

## Behave

You can install behave into your codebase by executing the following:

```bash
pip install git+https://github.com/behave/behave
```

To run the tests, simply execute `behave`.

#### Important: Directory Structure 

However, while running behave, there is almost an obvious problem with mapping the path of the code under test from the behave's test running context. Because behave runs each test under its own context, it was required to append the path to the code/service under test to the `sys.path` system variable like the following:

```python
from os import sys, path
sys.path.append(path.abspath('./pricing_service'))
```  

Pay attention to the directory structure. You may want to follow similar to below:

```bash
app
    features
        steps
            x_steps.py
        x.feature 
    services_or_controllers
        __init__.py
        x_controller.py
```  

#### Steps matching Specs

behave requires a mapping from specifications to the actual steps in order to test the code. Note here that it looks for the exact matching of the strings into the `given`, `when` and `then` steps. It can be parameterized as well just like the following.  

```python
@given('Customer specified from "{source}" to "{destination}" with "{pax}"')
def step_impl(context, source, destination, pax):
    context.source = source
    context.destination = destination
    context.pax = pax 

@when('She queries for price')
def step_impl(context):
    calculator = PricingCalculator()
    context.pricing = calculator.Calculate(context.source, context.destination, context.pax)

@then('She expects the pricing to be "{price}"')
def step_impl(context, price):
    assert context.pricing == int(price)
```

What is happening here is that behave is taking each row from the `Examples` specified in the feature file, and running it thru the `Scenario Outline`. For each line in `Scenario Outline`, it maps a matching `given`, `when` and `then` step. There is also a `context` variable which is used to carry intermittent variables and values to the subsequent steps. 

This sample can be downloaded from [here](https://github.com/tsaqib/samples/tree/master/bdd-behave).