Problem description:

  Goal: The primary goal of this project is to develop a win probability model for men's college basketball games that can predict the likelihood of each team winning in real-time as the game unfolds. This model seeks to enhance strategic decision-making for coaches, improve engagement for fans, and provide analysts with deeper insights into game dynamics.
  
  Background: In the realm of competitive sports, particularly college basketball, the ability to predict game outcomes accurately holds immense value. Coaches can optimize strategies based on likely outcomes, while broadcasters and fans can gain a richer understanding of the game as it progresses. Traditional static models, which do not account for events within the game, often fail to capture the full complexity of sports where momentum and psychological factors play a crucial role.
  
  Importance: Developing a win probability model that adjusts with the game's live data addresses this gap, offering a more nuanced and responsive tool. Such models can significantly impact areas ranging from live betting industries to media coverage, enhancing the analytical depth available during a game.
 
The key questions that we would like to address are:  
  How can we accurately predict the outcome of a game using both pre-game and in-game data? 
  What statistical methods best capture the complex dynamics of a college basketball game? 
  How can the model's predictions be used to influence decision-making during the game?
  What are the limitations of our probability model, and how can we overcome these moving forward?
  

Scraping Live Data:
The first task we had to accomplish was scraping the data from college basketball games and finding a way to scrape data from live games. We were able to accomplish scraping from live games by using the CBS play-by-play as unlike the ESPN play-by-play it is not a dynamic website.
We were able to scrape live data from the CBS play-by-play using the beautiful soup python package. From the games, we scraped the current time and score. To accomplish this we scrape the most recent data under the keywords “Time” and “Score.”  Using these numbers we created a simulation for the rest of the game.

Mathematical Model:
	To create a solution to predicting the outcome of a college basketball game at any point in the game we turned to simulating the rest of the game to predict a winner. The simulation is a Monte Carlo simulation with a Poisson-influenced random number generator. We simulate the rest of the game by creating a random next scoring time for each team and adding a point to the team with the least time to score. The random next scoring time is calculated similarly to the way we created arrays for inter-arrival times. The next scoring time for each team is calculated by taking a random number created using the numpy library and setting the parameter on it as the scoring rate for that team. The scoring rates are calculated by dividing their score by the time played.
The simulation works by creating a random number calculated using the scoring rate for each team to simulate the next time they score a point. This random number can be understood as how long it will take for a certain team to score one point. These two variables created, home_time and away_time, are then compared to each other, and the team with the shorter time to score is given a point. The time it took this team to score, denoted by either home_time or away_time,  is then subtracted from the time left in the game’s simulation. The simulation is repeated until the time left in the game is less than or equal to zero meaning the game is over.
Simulation’s Win Probability: The simulation is run ten thousand times to gather data to create a probability. The simulation’s probability is found by keeping track of the number of games played and the amount of times the home team won. The simulation’s probability of winning is calculated by dividing the number of wins for the home team by the number of games played. 

Colley Win Probability: To create a win probability before the game begins we use Colley rankings. The data used to create the rankins was scraped from the 2023-2024 season’s data from the Massey Ratings website. We use a weighted Colley program where home wins, away wins, and neutral wins are assigned weights of 1, 1.5, and 1.25 respectively. The season is then split where the first quarter in the season has a weight of 0.25, the second quarter has a weight of 0.5, the third quarter has a weight of 0.75, and the final quarter has a weight of 1. Once the Colley rankings are created we use code given to us by Dr. Chartier to create a probability for the home team to win the game before it begins. 

Overall Win Probability: The final probability is determined by the time left in the game, the simulation's probability, and the pre-game probability found by comparing Colley rankings. To calculate our final probability throughout the game we assign and manipulate weightings given the simulation and Colley probability using the time left in the game. 
In the first quarter of the game, the factor determining the weight of the probabilities is 0.2. To create our final probability we add the simulation’s probability multiplied by the factor and the Colley probability multiplied by one minus the factor; in the second quarter the factor is 0.4, in the third quarter it is 0.6 and in the final quarter of the game it is 0.8. This final probability is what our model returns to the user. 

