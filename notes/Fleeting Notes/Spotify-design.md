# Spotify-design
Created: 2023-05-08 22:33
Tags: 
____
* Constrain the problem to make it solvable within the hour
* encoding? movies and songs
* how downlods works? how to make something ready to download?
* bandwith?
* many to many? one to many?
* blob storage, object storge? why?
* why you split your database two difference
	* access pattern? type queries
* does that make scense
* try to be up front
___
functional requirements
1. finding music
2. playing music


non-functional requirements

4MB * 10^6 = 400 TB

* what features?
* Can ask you how much would be size of a single music?
	* 4 MB
* How many music would we have?
	* 10 M
* how many user do we have?
* how many users do thinking about here?
	* 1M
* how do we want search for a song?
* Does that make scense so fat?
* steam and network bandwith



? is that seems reasonable? is that seems ok?


#### Telegram

1. how many user do we have?
2. is just one-on-one chat or there is group chat
3. max limit of group?
4. messages are just text or users can send images and etc..?
5. there is limit for number of message chatacter?
6. how many days should we keep messages?
7. how many message people reguraly make in one day?



send_login_verfication(user, phone_number)
verify(user,phone_number, code)

send_message(user, message, receiver)
read_message(sender, from, size)


->  Dynamodb for chat application

