# escape-orbital
orbital

Team Name: Cacti

Proposed Level of Achievement: Apollo 11

Motivation: When you are looking for an escape or just in the mood for a different environment… We wanted to create a game that tells a tale in a tangible way, building a different reality to dive into, savour and ponder over. Being a survival game, players are challenged to be creative in the ways they stay alive while they navigate this sci-fi landscape, making the game replayable. 

Aim: We hope to make an immersive story-driven game that has a high replay value as well. 

Game Background: In a fictional country, the Republic of Avrane, is undergoing a series of reforms, which includes forcing the citizens to submit to dubious brain upgrades. The player finds this undesirable and decides to leave the country before this happens. Unfortunately, the authorities got wind of this plan and the player wakes up one morning to discover that the police are on their way. 
The player gathers as many items within the time limit, then sets off and tries to survive the journey to the border.

Features (link to game: https://drive.google.com/file/d/1zDBlYsgLz2j-gZXpU80eivTr5Dv4dgDu/view?usp=sharing)

1. Controllable characters: Avatar can be controlled to move and interact with the environment (pick up items, talk to NPC etc)
2. Game currency: Different forms of currency that are used up by actions, and vulnerable to events. This is implemented by assigning the total amount of currency the player holds at any point of time to a static variable.
3. Energy Bar: Energy starts from 100 and depletes at a rate of -6%/hr under normal circumstances. Energy is increased by food, medicine and sleep, and depletes more with random events. As players interact with the NPCs, or purchase food or services that require time, when the function, HealthBar.ChangeHealth(float timeTaken) is called. This function takes in the time taken to perform the action as the argument, and calculates the amount of energy depleted. As mentioned above, this is implemented by assigning the total amount of energy of the player at any point of time to a static variable. Once the energy bar reaches 0%, the player loses the game.
4. Random events: Injuries and sickness are random events gotten between locations.:

	- Sprained ankle -9% Energy /hr, 
	-	broken arm (bleeding) -12% Energy /hr 
	-	Food poisoning-9% Energy /hr 
	-	Flu: -6% Energy /hr
	-	Find money: +$18  

	The rate of energy lost is in addition to the -6%/hr lost under normal circumstances. Each action will then result in a higher energy expense. This is implemented by using static boolean variables which returns true when the player is injured or sick. Therefore, when HealthBar. ChangeHealth(float timeTaken) is called, the additional energy costs would be calculated, resulting in a higher energy depletion rate.

	Sprained ankle and broken arm can be healed in the hospital. 
Food poisoning and flu can be cured by purchasing the stomach pills and fever pills respectively from the supermarket or the hospital. This is implemented by changing the static boolean variables to false.

	We implemented random events using a random number generator, generating number 1 to 6, with each number representing a different event and the last number representing no event occurring. Random events will occur when the player travels to another city. 

5. Food, medicine and rest:	Food and medicine can be taken from the house or bought in the city, from different shops:
	- Snacks +5% Energy
	- Meal +15% Energy
	- Energy pills +50% Energy
	- Super energy pills +100% Energy
	
	Rest can be gotten at:
	- Motels, +20% Energy/hr
	- Bench, +20% Energy/hr

6. Money:	Goods and services in the shop cost money (see below). Money can be gotten from the house and from random events (see above) while travelling. This is done by calling the Money.takeMoney(int amt) function, which takes the amount taken as argument and adds it to the static int variable which is the current amount of money held by the player. Once the static int variable is updated, the UI text indicating the money held by the player will be assigned to the toString() value of the static int variable.

	The player would not be able to purchase items with insufficient money. This is checked using an if condition: if (moneyNeeded < moneyPlayerHas) then an UI indicating a failed attempt to purchase the item will pop up, else, another UI indicating a successful purchase will appear.

7. Time:	Actions cost time; the player can only stay in one location for up to 6h before the police arrive.
	- Each purchase takes 20 min (which is equivalent to 2% of energy cost)
	- Resting in motels is booked by the hour
	- Services in the hospital take 1h each
	- Services on the player's phone take 40 minutes each.
	- Services in the PC cafe take 20 minutes each.
	- Travelling takes 30 mins per km
	- Interacting with each npc takes 10 minutes each.

8. Suspicion Bar:	Each action will cost the suspicion on the player to grow. Purchasing items, interacting with NPCs, using services, will each cost +5% suspicion on the player. Actions such as sleeping on the bench and digging through trash will cost +10% suspicion. Similar to the energy bar and money, suspicion bar is implemented using a static int variable. 

	The suspicion level is reset to zero whenever the player travels to a new town.

	Whenever an action is being taken by the player that causes the suspicion level to increase, there will be a probability corresponding to the suspicion level whereby the police would have caught up to the player, resulting in a pop up of a countdown timer, to warn the player to leave the town within 90 seconds, if not, player would lose the game.

	This is implemented by using a random number generator, Random.value, which generates a random number from 0.0 to 1.0. By using an if condition: if (Random.value > 1 - (suspicionLevel/100)), which indicates that the probability of satisfying this condition is equal to the suspicion level, then the countdown would be triggered. 
3	Special items	Special items give players leverage, with certain costs. Special items can be taken at the start of the game in the house, or during the game journey. Special items include:
	- Car keys
	- Contact lens
	- Fake passport

9. Car keys.	Advantages: time taken to travel between each town is reduced to 5 mins per km, which is highly advantageous as compared to the original 30 mins per km. Players will then save on their energy level.

	Disadvantages: the player starts out with a higher suspicion level of 20%, as compared to the original 0% whenever the player travels to a new town, since the car can be tracked down by the police.

	This can be implemented using a static boolean value to indicate whether the car keys are taken or not, by true and false respectively.

10.	Contact lens.	Advantages: certain actions will require less time to execute, such as purchasing items.

	This can be implemented using a static boolean value to indicate whether the contact lenses are taken or not, by true and false respectively.

11.	Fake passport: It is a requirement for the player to be in possession of a fake passport to win the game. Without a fake passport, players will not be able to cross the border.

12.	Distance: Player starts 80 km away from the border. The distance away from the border is assigned to a static int variable. Travelling from location to location reduces the distance to the border by 5 km to 10 km each time (random). Similar to the random events, this is implemented using a random number generator, generating a random number from 5 to 10. After which, the ChangeDistance(int dist) function is called to change the value of the static int variable assigned to the value of the distance, hence updating the total amount of distance left.

13.	Different types of locations.	Different locations offer different amenities, which grants access to different interactions and resources which includes:
	- Interactions with NPCs that might give the player major hints, and activating certain services such as being able to unfreeze the bank account: player will be able to gain access to a large amount of cash, being able to make a fake passport which is essential to win the game, and being able to disable the car tracker which will disable the +20 Suspicion level whenever the player travels to a new town.
	- Goods that can increase energy of the player
	- Services that provide aid to the player in terms of information and energy.

14. Pharmacy.	Goods: Energy Pills ($30), Super Energy Pills ($50)

15. PC cafe.	Goods: Snacks ($5), Meals ($10)

	Services: Use PC ($10/h) (Book Motel, Gather Information)

16.	Supermarket.	Goods: Snacks ($5), Meals ($10)
17.	Motel.	Services: Rest ($10/h)
18.	Cafe.	Goods: Snacks ($5), Meals ($10)
19.	Hospital.	Goods: Energy Pills ($30), Super Energy Pills ($50), Fever Pills ($30), stomach pills ($30)

	Services: Bandage Ankle ($40), Bandage Arm ($50)
	
20.	Charging Station.	The players will be able to recharge their electric car if they had taken their car.

	Services: Recharge electric car ($20 per 10% charge)

21.	Storyline.	Players will receive story updates in the form of pop ups when they travel a certain distance. 
	- < 65 km away from the border
	- < 50 km away from the border
	- < 35 km away from the border
	- < 20 km away from the border
	
	Since each town is 5 - 10 km away from one another, players will not miss any story updates.

22. Account Creation.	Players are able to register for an account to save their progress and access the leaderboard.
23.	Leaderboard.	Players would be able to compete with other players on the leaderboard according to how well they achieve their goal
24.	Account Statistics.	Players are able to check their game statistics such as the number of times the player fails or succeeds in reaching the border and the average number of days taken to reach the border.
25.	Fail count	The number of times player has failed to reach the goal of the game
26.	Success count	The number of times player successfully reached the end goal of the game

Incompleted Features

	Due to unforeseen bugs and other circumstances, we were not able to complete some features of our project before the deadline. However, we will detail the steps we have taken to implement them and what issues we could not overcome.
	- Creation of online database
	- We used PHP and MySQL to create an online database, which holds the list of users and all the relevant information for account creation and account statistics (ie userid, username, password, wincount, deathcount, totaltime taken). 
	- The server side scripts were done using PHP and the information from the database would be retrieved and displayed in game using UnityWebRequest. 
	- However, we only managed to implement this database on localhost (using Xampp), and faced errors using an online server, even after changing host.
	- Since we did not manage to overcome this bug in time, we did not implement the account statistics. Login page and Register User page (that are no longer in game)
