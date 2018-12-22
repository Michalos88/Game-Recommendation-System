# Steam Game Recommendation System

We developed simple yet effective game recommendation system using SVD with tresholding.

#### Data Set 
From Kaggle: "Steam is the world's most popular PC Gaming hub. With a massive collection that includes everything from AAA blockbusters to small indie titles, great discovery tools can be super valuable for Steam. How can we make them better?
This dataset is a list of user behaviors, with columns: user-id, game-title, behavior-name, value. The behaviors included are 'purchase' and 'play'. The value indicates the degree to which the behavior was performed - in the case of 'purchase' the value is always 1, and in the case of 'play' the value represents the number of hours the user has played the game."

https://www.kaggle.com/tamber/steam-video-games


This dataset is a list of 200,000 user behaviors, with columns: user-id, game-title, behavior-name and play_time.. The behaviors are divided into 'purchase' and 'play', which indicates if the record constitutes to a purchase receipt or user interaction with the game. The play_time feature indicates the degree to which the behavior was performed - in the case of 'purchase' the value is always 1, and in the case of 'play' the value represents the number of hours the user has played the game. There are total 12393 unique user ids and 5155 games. On average Steam purchases 10 games and plays each games at least 48 hours.

#### Approach:
We mapped the data to a joint latent factor space of dimensionality d*n, such that the user-item interactions are modeled as inner products in the space. Each user is associated with a vector pu Rd and each item is associated with a vector qiRn. 
For a given game i, the elements of qi measure the extent to which a game was played by users  pu. Similarly, for a given user u, the elements of pu measure the extent of interest the user has in the game (in this case we have a binary value: 1 for purchase, 0 for not purchased). 
The resulting dot product qiTpu captures the interaction between the user u and the game i. 
We get an approximation of the user u’s rating of a game i which is denoted by:
                                                                                r^ui= qiTpu                            (1)

The model closely represents Singular Value Decomposition. At a high level, SVD is an algorithm that decomposes a matrix M into into two unitary matrices (U and Vt) and a diagonal matrix S:
M=USVT       (2)
where M is user-game purchases matrix, U is the basis matrix, S is the diagonal matrix of singular values (essentially weights), and VT is the “features” matrix. U represents how much users “like” each feature and VT represents how relevant each game is to the user.
A sparsification technique is then applied to approximate  the rank of matrix M, namely the authors utilized thresholding by parameter “k” on the diagonal matrix S to remove not-meaningful representations.  Furthermore the authors tuned the parameter “k” to increase or decrease the number of r user-havioral groups. 
Then authors recomposed M utilizing equation (2) to obtain final recommendation matrix R. 

#### Testing and Results:
For testing the recommendation engine the authors used random uniform sample of 2000 unique users who have purchased and played minimum of “d” games in other to reduce intrinsic randomness of recommending a single game based on a single feature. We made sure that all 2000 users selected for testing are unique. Then we selected “n” random game for every user in the testing group and altered their behavior by removing those “n” game purchase from the main matrix R. Then the authors checked, if the removed games appeared in the recommended set of “f” games. Then the mean of all of the recalls has been computed to arrived at global recall. Finally through hyper-parameters (k, n, d, f) tuning the authors arrived at global recall of  28% by setting k=101, n=2, d=10, f = 100 . (k value of 101 allows for rank of 23).

