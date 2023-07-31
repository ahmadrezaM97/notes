# framework
Created: 2023-01-11 00:20
Tags: 
____

#### Start by Clarifying Requirements

you will be given some incredibly vague problem, like "Design YouTube"
It is up to you to turn this into concrete requirements your system must meet.

Start by repeating the question and confirm you understand it with the interviewer.

__ASK LOTS OF QUESTIONS, THINK OUT LOUD__

1. repeating question
2. what parts of youtube? ask for features
3. how much viedos we are talking about?
4. working backwards
	1. Start from the customer experience to define your requirements
	2. this will gain MAJOR POINTS at Amazon, but works in general.
5. how will users discover videos? Do we need to think about building a search engine? A recommender engine? and advertising engine?
	1. Use this to limit the scope of what you're being asked to do
	2. Understand the `customer experience` you are being asked to deliver
	3. Identify WHO are the customer
	4. WHAT are their use cases
	5. you'r not going design all of youtube in 20 minutes
6. your initial task is to __CLARIFY THE REQUIREMENTS__ of what you are designing perspective and not a purely technical one.


Defining scaling?

1. nail down the scale of the system, is it hundreds of users? milions?
	1. This will inform you on the need for horizontal partitioning
	2. how often are users comming? what transaction rate do you need to suppert?
2. Also define the scale of the data
	1. hundererd of viedos? miliion of videos?
	2. you will need every trick in the book for horizontally scaled servers and data storage
3. some internal too might not need this level of compexity, hoever
	1. always prefer the simplerst solution that will work
	2. vertical scaling still has its place
4. defining latency requirements
	1. this informs the need for cacheing and cdn usafe
	2. caching is also a tool for scaling, howeever it reduces load on services and & datasores
	3. try to exoress this in SLA languange 
		1. 100ms at thee-nines for given operation
	4. caching video recommendations
	5. cacheing video metadata, descriptions
5. 
_____
##### References
1.

