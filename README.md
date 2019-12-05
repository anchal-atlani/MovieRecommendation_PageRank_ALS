# MovieRecommendation_PageRank_ALS


## Movie Recommendation Using Personalized Page Rank and Matrix Factorization using ALS

## Team Members
  * Anchal Atlani - aatlani@uncc.edu 801084129
  * Srishtee Marotkar - smarotka@uncc.edu 801084153
  
## Overview
To design and compare two movie recommendation systems for a user based on self or similar user’s likings. We will be implementing two recommendation systems, first Using the Personalized PageRank algorithm and second using the Matrix Factorization using Alternate Least Square algorithm on the distributed computing environment.

## Motivation

Providing relevant recommendations has helped users to explore their area of interest and has proved to be an evident factor in retaining customers. Considering the explosion of data due to the vast number of users and movies not only the efficient algorithms are necessary but also computation should be done in a distributed fashion in order to get results in the blink of an eye.

## Fun Factor
After learning Page Rank algorithm and Hubs and Authority we came read papers on these topics and found out that we can combine both the approaches to generate recommendations.Consider that we have a set of users and a set of movies that each user enjoyed watching and this movie is watched by a few other users as well. A movie is relevant to me if other similar users enjoyed it. A user is similar to me in case they like movies that are relevant to me. We can use this interdependent relationship graph between users and movies to get rankings of relevant movies using the Personalized PageRank algorithm. 

## Task Involved
  
  * Data Preprocessing:
  * Implementation of Personalized Page Rank Algorithm 
  * Implementation of Matrix Factorizion using ALS
  * Execution of Algorithms on AWS EMR
  * Analysis of Results
  * Report Preparation
  

## Approach
* **Personalized Page Rank:**
  For implementing Personlized Page Rank based recommendation system we have used the concept of Page Rank and Hubs and         Authority.
  
  We have referred https://web.stanford.edu/class/msande233/handouts/lecture8.pdf paper for implementation. In this approach we are taking user-product interaction and try to recommend movies to the user based on the liking of similar user with a given user.
  
       rank of movie = sum(rank of users who watched this movie/ total number of user watched the movie)
       rank of user = sum (rank of movies watched by the user/ total number of movies watched by the user )
  
 
        rank of movie = sum(rank of users who watched this movie/ total number of user watched the movie)
        rank of user = (1-e)*sum (rank of movies watched by the user/ total number of movies watched by the user ) + e (if                           user =u)
        
        Here, e is teleportation factor and it is added when we match a user id of the user for which we want to find the recommendation.
        

 * **Matrix Factorization using ALS:**

   We have implemented Matrix Factorization using the Alternate Mean Square method. In this, we are first creating a sparse matrix that will have rating information on the movie watched by the users.

   For Generating the factorized matrix we have used the ALS method.  To run this algorithm on a distributed environment we have referred the paper "CME 323: Distributed Algorithms and Optimization, Spring 2015 
http://stanford.edu/~rezab/dao.Instructor:  Reza Zadeh, Databr " .

  In this first, we have created a Rating Matrix having original ratings and initialised two matrices with random values 
   corresponds to the P and Q matrix.

  In each iteration as per the ALS method we are keeping first P constant and trying to predict values for Q and then 
  keeping Q constant and trying to predict values for P. Since each row updation of each matrix is independent of each 
  other and hence they can be updated independently and then we can get a overall matrix.

               Minimize objective fuction
                p= (sum(QTQ) + lambda* I)^-1* sum(rating_ui * Q)
                q= (sum(PTP) +lambda *I)^-1 * sum(rating_ui * P)
                  
 ## Pacakages and Language Used 
   * python, nltk, spark, AWS EMR, S3, numpy
## Step Implemented

   * In Page Rank we have taken complete corpus and we got 9201 unique movies and 470860 unique users.
   * For both the algorithm we have taken only movies which are rated 4 and above as in Page Rank we are considering to recommend movies which are liked by other users and liking we are defining in terms of rating.
   * For Page Rank Algorithm we have just read the data file and converted a link graph out of it.
   * After the complete eecution of Page Rank we are mapping the movie id with thier respective titles.
   *	For the matrix factorization since the customer id were not in sequence so we have created a label mapping for each customer id as indexes
   *  The data for matrix factorization is huge and due to hardware limitation we have removed the data having user id frequency less that 50 in overall corpus. Apart from this we have limited the number of unique movies to 3000
   * For implementaion of ALS in distributed environment we have rating matrix of size 86059*3000. This matrix is sparse matrix and used by RDD for minimization. Since the matrices are large we have used the Broadcast approach as mentioned in the paper. It makes copy of matrices at every node. 
  

## Final Product
 * Implemented two algorithms Personalized Page Rank and Matrix Factorization using ALS for movie recommendation system.
 * After the complete iterations we have given the option for calculation of rating for each movie for a specific user.
 * As a final product of Personlized Page Rank we can pass any user id and let algorithm compute the recommendations.
 * As a final product of Matrix Factorization algorithm can recommend a movie based on the predicted ratings.
 
 
## Results

  ### Movie Recommendation using PageRank and Hubs and Authority
  * **Personlized Movie Rankings**
  Tested for userid  '1110480', the results are below for top 200 movies.
  
  * **Observation**: 
   * The top movies contains first the movies which the user rated from this we conclude that the personalization is taking effect as the     movies which was watched by that user got the higher ranking compare to other movies. In our test case below the user has watched       180 movies and thus those 180 movies got high ranking. So, to recommend a new movie to user we will peek out first his watched           movies and all the movies there after are the recommended movies. 
   
   * Also the ranking of movie is not depending on how many users have watched the movie but it does depend on the user for which we are searching the recommendation. This is important observation to understand how we can personalize the recommendation using teleportation.
  
  
    This can be verfied by seeing the link graph of customer 
  * **Link Graph: (UserId, [movies watched by user id], (rank=1/ (total number of users) )**
  
  * **('1110480', (['28', '30', '46', '187', '191', '241', '290', '297', '311', '312', '313', '329', '331', '353', '357', '393', '471', '482', '534', '571', '607', '645', '758', '788', '886', '897', '989', '1039', '1102', '1180', '1250', '1428', '1470', '1503', '1509', '1590', '1673', '1719', '1754', '1770', '1865', '1905', '1962', '1974', '2015', '2016', '2095', '2122', '2172', '2342', '2372', '2395', '2452', '2462', '2465', '2470', '2519', '2699', '2743', '2862', '2874', '2940', '3151', '3197', '3282', '3368', '3371', '5085', '5181', '5226', '5227', '5231', '5237', '5401', '5472', '5496', '5522', '5538', '5582', '5601', '5658', '5695', '5762', '5788', '5814', '5862', '5926', '5963', '6029', '6034', '6040', '6042', '6099', '6134', '6195', '6219', '6274', '6329', '6337', '6350', '6366', '3446', '3510', '3593', '3624', '3648', '3684', '3713', '3782', '3817', '3860', '3875', '3905', '3917', '3938', '3954', '4043', '4141', '4227', '4345', '4384', '4432', '4488', '4586', '4588', '4641', '4660', '4727', '4745', '4829', '4890', '4906', '4912', '4951', '5020', '6450', '6464', '6473', '6475', '6555', '6596', '6736', '6796', '6850', '6872', '6874', '6953', '6960', '6974', '7237', '7281', '7363', '7511', '7513', '7517', '7586', '7635', '7683', '7701', '7786', '7904', '7928', '7971', '8045', '8131', '8197', '8393', '8469', '8619', '8644', '8743', '8782', '8806', '8832', '8846', '8872', '8966', '9051', '9170', '9175'], 2.1105684605091537e-06))**
  
  
  * **Recommended Movie : (Movie_id, Title, Rank, Total Users watched the movie) :**
   Recommended movies starting are highlighted as bold, also these movies are watched by the users who watched the movies watched by this user.
      ####
      ##### Movies Watched By user #######
            ('1905', 'Pirates of the Caribbean: The Curse of the Black Pearl', 0.0055328796947541675, 153325)
            ('2452', 'Lord of the Rings: The Fellowship of the Ring', 0.005418893550783347, 129794)
            ('571', 'American Beauty', 0.00541744969469533, 111768)
            ('3938', 'Shrek 2', 0.0053476763107867375, 120442)
            ('2862', 'The Silence of the Lambs', 0.005317153860358559, 110815)
            ('5862', 'Memento', 0.005290533988378891, 87356)
            ('6974', 'The Usual Suspects', 0.005285943810776255, 93614)
            ('5926', 'Fight Club', 0.005268200498662075, 89617)
            ('8644', 'Catch Me If You Can', 0.005257354738703196, 95126)
            ('3624', 'The Last Samurai', 0.005256840233365211, 100481)
            ('886', 'Ray', 0.005252307248509024, 87971)
            ('2372', 'The Bourne Supremacy', 0.005252109371977359, 97246)
            ('4432', 'The Italian Job', 0.005242241301973718, 100146)
            ('8782', 'The Royal Tenenbaums', 0.005207089996595575, 68968)
            ('1865', 'Eternal Sunshine of the Spotless Mind', 0.005198153409599958, 68043)
            ('6134', 'Collateral', 0.005196576244952842, 81875)
            ('6029', 'Amelie', 0.005192312264014017, 65966)
            ('5085', 'Seabiscuit', 0.005192234749393993, 83819)
            ('5496', 'I', 0.005192066948060923, 96345)
            ('1962', '50 First Dates', 0.005183711169377038, 91442)
            ('2122', 'Being John Malkovich', 0.005181639215383148, 65860)
            ('1180', 'A Beautiful Mind', 0.005167193346853692, 80897)
            ('5582', 'Star Wars: Episode V: The Empire Strikes Back', 0.005147032199840427, 83744)
            ('3282', 'Sideways', 0.005146827561875174, 60836)
            ('30', "Something's Gotta Give", 0.005145748799257023, 76743)
            ('3917', 'Garden State', 0.005144141814847265, 59796)
            ('2342', 'Super Size Me', 0.0051439409677952255, 62605)
            ('4345', 'Bowling for Columbine', 0.005143008982881507, 61044)
            ('1470', 'Bend It Like Beckham', 0.005135490307215131, 65091)
            ('3151', 'Napoleon Dynamite', 0.0051175305131443815, 59991)
            ('6099', 'Apocalypse Now', 0.00510793519440347, 61832)
            ('3860', 'Bruce Almighty', 0.0051028296078673855, 77092)
            ('3371', 'Whale Rider', 0.005093173990850881, 53424)
            ('191', 'X2: X-Men United', 0.005084735894184535, 70639)
            ('4951', 'Runaway Jury', 0.005073933900980581, 65983)
            ('6475', 'Spanglish', 0.005057928369758418, 56534)
            ('5401', 'Dodgeball: A True Underdog Story', 0.005047757616271593, 56691)
            ('2743', 'The Pianist', 0.00504099661664355, 48580)
            ('758', 'Mean Girls', 0.005034072125908778, 53674)
            ('6274', 'The Hunt for Red October', 0.005033164185181725, 65199)
            ('5226', 'Rushmore', 0.005028548362744841, 41070)
            ('5181', 'Under the Tuscan Sun', 0.0050219552434340245, 54022)
            ('1428', 'The Recruit', 0.005021711463913226, 61272)
            ('313', 'Pay It Forward', 0.005018529369924631, 60724)
            ('8832', 'Citizen Kane', 0.005018445246701351, 45157)
            ('6329', 'Edward Scissorhands', 0.005016535686576608, 49971)
            ('357', 'House of Sand and Fog', 0.005016124933625221, 42348)
            ('9051', 'Caddyshack', 0.005006480980436919, 52759)
            ('8393', 'Ladder 49', 0.005003835752153963, 56799)
            ('7281', "Monster's Ball", 0.0049991397861722025, 43143)
            ('312', 'High Fidelity', 0.004995300817527838, 40433)
            ('482', 'Frida', 0.0049911668376747724, 37313)
            ('8743', 'Ice Age', 0.004990582360604067, 55547)
            ('2874', 'Elf', 0.004989884998402019, 45275)
            ('5788', 'Sling Blade', 0.004987195281163086, 43046)
            ('5762', 'Almost Famous', 0.004983837659140777, 41035)
            ('7786', 'The Station Agent', 0.004983791155785453, 31562)
            ('6450', 'City of God', 0.004980483723808097, 33444)
            ('2465', 'This Is Spinal Tap', 0.004975612550324161, 36695)
            ('8846', 'Election', 0.004974779807748887, 35456)
            ('4043', 'Signs', 0.004973531450614622, 48108)
            ('1754', 'Sixteen Candles', 0.004970257505082073, 47398)
            ('607', 'Speed', 0.0049640899476735435, 53099)
            ('788', 'Clerks', 0.004960636238785987, 37476)
            ('1102', 'Training Day', 0.004949959910822933, 41432)
            ('4745', 'Bringing Down the House', 0.004942891314182772, 46364)
            ('329', 'Dogma', 0.004941954348579214, 37435)
            ('5472', 'Spaceballs', 0.004940365379221946, 40901)
            ('7635', 'Anchorman: The Legend of Ron Burgundy', 0.004940061038808556, 34536)
            ('241', 'North by Northwest', 0.004938209003426928, 34906)
            ('2095', 'Liar Liar', 0.004937983656520673, 46266)
            ('3905', 'The Others', 0.004930506057690767, 36637)
            ('6350', 'Rush Hour', 0.00492753190660363, 44538)
            ('4727', 'Apocalypse Now Redux', 0.004927152371778475, 32655)
            ('7511', 'Blade', 0.004924547403761839, 42947)
            ('3368', 'The Family Man', 0.004917824180277118, 40904)
            ('3875', 'The Motorcycle Diaries', 0.004916689858045717, 23948)
            ('6366', 'Saved!', 0.0049151079305914044, 27170)
            ('4227', 'The Full Monty', 0.004914690080235806, 31394)
            ('5538', "Monty Python's Life of Brian", 0.004913478802666611, 30502)
            ('3446', 'Spirited Away', 0.004909929605796818, 27776)
            ('7928', 'Antwone Fisher', 0.004907668960018435, 33125)
            ('7904', 'Punch-Drunk Love', 0.004907630670436778, 24097)
            ('7513', 'The Life of David Gale', 0.004906254423914149, 30766)
            ('7971', 'In the Bedroom', 0.0049041693680756185, 24242)
            ('6042', 'Starsky & Hutch', 0.004901856821635109, 31256)
            ('1590', 'Life as a House', 0.0049016945282441705, 30614)
            ('7586', 'The Bridge on the River Kwai', 0.004900602485769235, 30887)
            ('8806', 'The Good', 0.004898450758255677, 31974)
            ('290', 'Harold and Kumar Go to White Castle', 0.004897261595002744, 26852)
            ('1719', 'The Life Aquatic with Steve Zissou', 0.004895500656104899, 22952)
            ('331', 'Chasing Amy', 0.004893047889357762, 27393)
            ('9170', 'Ghost World', 0.004889512523311808, 20950)
            ('5695', 'Bad Santa', 0.0048889949295668235, 26278)
            ('3713', 'Saw', 0.004888218794595364, 29428)
            ('5601', 'The Manchurian Candidate', 0.004884473817391185, 25092)
            ('4829', 'Shaun of the Dead', 0.004883814894314233, 22920)
            ('2462', 'Planes', 0.004881595438725952, 29772)
            ('2470', "Wayne's World", 0.004880947246910638, 29309)
            ('6464', 'Unfaithful', 0.004879797002989969, 26889)
            ('2015', 'Talk to Her', 0.0048758017540373325, 18146)
            ('3684', 'Goldfinger', 0.004871987607509537, 28861)
            ('2016', 'The Magdalene Sisters', 0.004871082111655026, 18286)
            ('6555', 'Insomnia', 0.004869240101836712, 24368)
            ('2699', 'The Missing', 0.004867206343195336, 24894)
            ('6874', 'The Cooler', 0.004861196093558704, 17881)
            ('5814', 'One Hour Photo', 0.004860416617116803, 21385)
            ('8966', 'Pieces of April', 0.004859788350086652, 16401)
            ('4488', 'Wonder Boys', 0.004854035611814624, 18109)
            ('8045', 'Open Range', 0.004850303322296962, 21854)
            ('28', 'Lilo and Stitch', 0.004849176827163199, 26084)
            ('1770', 'Trainspotting', 0.004846590220682143, 17182)
            ('7683', "Cinema Paradiso: Director's Cut", 0.004842407975912369, 15783)
            ('3648', 'Who Framed Roger Rabbit?: Special Edition', 0.004842171765149185, 22902)
            ('4141', 'Shanghai Noon', 0.004840150555229019, 24063)
            ('1974', 'Il Postino', 0.004835863939370511, 15265)
            ('1509', "National Lampoon's Van Wilder", 0.004833560370712724, 21695)
            ('2172', 'The Simpsons: Season 3', 0.004832841088476837, 19408)
            ('7701', 'The Road Warrior', 0.004831521963005667, 19967)
            ('7517', 'Before Sunrise', 0.0048291294585182595, 12754)
            ('3782', 'Flatliners', 0.0048281608359857205, 21960)
            ('2395', 'Scream', 0.00482800616429282, 20954)
            ('4384', 'Dawn of the Dead', 0.0048257643322352365, 16925)
            ('311', 'Ed Wood', 0.004822873841298766, 13936)
            ('6195', 'The Woodsman', 0.004812709883332612, 9973)
            ('3817', 'Stargate', 0.0048121434427065134, 19182)
            ('4660', 'The Shipping News', 0.004811347646950981, 12142)
            ('6736', 'Robots', 0.00481129555998464, 15865)
            ('7237', "Monty Python's Flying Circus", 0.0048112954947676844, 13672)
            ('5522', 'The Machinist', 0.004808586559575205, 10491)
            ('8619', "Pee-Wee's Big Adventure", 0.004808023015711898, 13140)
            ('5237', 'To Die For', 0.004801643146221156, 11249)
            ('2519', 'A Love Song for Bobby Long', 0.00480098949423416, 8981)
            ('6596', 'Beyond the Sea', 0.004800758228006171, 9313)
            ('8197', 'Deadwood: Season 1', 0.00479961254647147, 10282)
            ('4906', 'Stuart Little', 0.004792331561773145, 14978)
            ('3197', 'Around the World in 80 Days', 0.004791973215624979, 12350)
            ('8131', 'The Conversation', 0.004789155091662388, 8690)
            ('6034', 'Open Water', 0.004787728479554024, 7331)
            ('989', 'The Door in the Floor', 0.004785092271195446, 6119)
            ('3954', 'The Quiet Man', 0.00478366798166073, 10612)
            ('645', 'Dear Frankie', 0.004783518561171846, 6386)
            ('6219', 'The Basketball Diaries', 0.004782708594640705, 8911)
            ('4912', 'Mister Roberts', 0.004781086124482853, 10526)
            ('6850', 'Lords of Dogtown', 0.004779728364457708, 6385)
            ('6872', 'Greenfingers', 0.004779627977693847, 6458)
            ('4586', "But I'm a Cheerleader", 0.0047790335813730905, 7194)
            ('6337', 'Joe Dirt', 0.004778589376610543, 10642)
            ('897', 'Bride and Prejudice', 0.0047729226604528805, 5399)
      
      ##### END Movies Watched By user #######
      
      
      #### Start of Movies Recommened to user #####
            ('6040', 'The L Word: Season 1', 0.004772589562512087, 5121)
            ('187', 'Death to Smoochy', 0.0047708906878448035, 6980)
            ('7363', 'Moonlight Mile', 0.004769586092009992, 4891)
            ('353', 'Life or Something Like It', 0.004767605060368781, 7924)
            ***('6960', 'The Goodbye Girl', 0.00476682425335963, 7621)
            ('3593', 'My Neighbor Totoro', 0.004762289048192278, 4256)
            ('8872', 'In & Out', 0.004760867143566023, 6650)
            ('3510', 'Hard Eight', 0.004760346406727629, 3949)
            ('5963', 'The Gold Rush', 0.004757631329500506, 3949)
            ('5227', 'Angel Eyes', 0.004756342673333938, 5888)
            ('471', 'City Lights', 0.004755937154242396, 3409)
            ('5658', 'The Experiment', 0.004755110352477394, 2845)
            ('5020', 'Johnny English', 0.004754397033201961, 3819)
            ('1673', 'Defending Your Life', 0.004751535767176584, 3807)
            ('6796', "Jesus' Son", 0.004751209797157079, 2487)
            ('46', 'Rudolph the Red-Nosed Reindeer', 0.004751190249593268, 4192)
            ('1250', 'Brassed Off', 0.004750780132851685, 3067)
            ('4641', 'Tully', 0.004750389262262187, 1806)
            ('5231', 'The General (Silent)', 0.004749653817539578, 2757)
            ('9175', 'The Shootist', 0.004749259499783169, 4835)
            ('393', 'The Replacement Killers', 0.0047490020249229044, 4947)
            ('4890', 'Next Stop Wonderland', 0.004748935071139906, 2290)
            ('534', 'With a Friend Like Harry', 0.00474877566634552, 2158)
            ('2940', 'Existenz', 0.0047485603764719836, 3072)
            ('8469', 'Living in Oblivion', 0.004746099369690235, 1796)
            ('1503', 'The Boxer', 0.004741355976539214, 1774)
            ('297', 'The Hebrew Hammer', 0.004740460525520532, 590)
            ('6473', 'Cherish', 0.004739342702578909, 970)
            ('6953', 'Twin Falls Idaho', 0.004739027918707338, 1125)
            ('1039', 'Lawn Dogs', 0.004737580418034673, 731)
            ('4588', 'Proof', 0.004735340958130598, 362)
            ('4306', 'The Sixth Sense', 0.0006533349044828153, 130869)
            ('3962', 'Finding Nemo (Widescreen)', 0.000634940784296544, 123889)
            ('6037', 'The Bourne Identity', 0.0006177111628433697, 118374)
            ('6287', 'Pretty Woman', 0.000571517157768216, 133429)
            ('2782', 'Braveheart', 0.0005472513373792445, 112533)
            ('8904', 'Good Will Hunting', 0.0005165016139482023, 97794)
            ('1220', 'Man on Fire', 0.0005133693485403251, 99911)
            ('6971', "Ferris Bueller's Day Off", 0.0005092625135987567, 96068)
            ('457', 'Kill Bill: Vol. 2', 0.00048006755012402265, 80293)
            ('6206', 'My Big Fat Greek Wedding', 0.00047896448873693766, 91301)
            ('4640', 'Rain Man', 0.00047768366443762223, 97305)
            ('2913', 'Finding Neverland', 0.000471269630182149, 79177)
            ('5317', 'Miss Congeniality', 0.0004484841656895042, 109557)
            ('7624', 'Top Gun', 0.0004473289071160703, 100364)
            ('8327', "One Flew Over the Cuckoo's Nest", 0.00043685745709002254, 73522)
            ('4656', 'Erin Brockovich', 0.0004287505414144158, 86939)
            ('5732', 'GoodFellas: Special Edition', 0.0004286938022406422, 75669)
            ('8596', 'Seven', 0.0004252666035265082, 79319)
            ('175', 'Reservoir Dogs', 0.00041111222620147296, 67030)
            ('7234', 'Men of Honor', 0.0004057174014024275, 99597)**

  #### Global Movie Rankings
  * **Observation**:
    * As this can be seen from below result that if we run the algorithm without teleporting for any user we get global ranking of             movies recommendations or we can say that these are mostly watched movies.
        
                ('1905', 'Pirates of the Caribbean: The Curse of the Black Pearl', 0.0053312195921993415, 153325)
                ('6287', 'Pretty Woman', 0.004628289591945733, 133429)
                ('4306', 'The Sixth Sense', 0.004550508258975815, 130869)
                ('2452', 'Lord of the Rings: The Fellowship of the Ring', 0.004517278155593091, 129794)
                ('3962', 'Finding Nemo (Widescreen)', 0.0043080967228621885, 123889)
                ('3938', 'Shrek 2', 0.004185204915260658, 120442)
                ('6037', 'The Bourne Identity', 0.004114854200181161, 118374)
                ('2782', 'Braveheart', 0.003912507125383093, 112533)
                ('571', 'American Beauty', 0.003895559377857228, 111768)
                ('2862', 'The Silence of the Lambs', 0.0038578250011688576, 110815)
                ('5317', 'Miss Congeniality', 0.0037976719972958317, 109557)
                ('3624', 'The Last Samurai', 0.0034923999334564937, 100481)
                ('7624', 'Top Gun', 0.003484262960921744, 100364)
                ('4432', 'The Italian Job', 0.0034784984383226265, 100146)
                ('1220', 'Man on Fire', 0.0034691400089838357, 99911)
                ('7234', 'Men of Honor', 0.0034525542479934847, 99597)
                ('8904', 'Good Will Hunting', 0.003402638748863022, 97794)
                ('4640', 'Rain Man', 0.0033835791660699914, 97305)
                ('2372', 'The Bourne Supremacy', 0.00337994532478872, 97246)
                ('6972', 'Armageddon', 0.0033648842135613997, 97097)
                ('5496', 'I', 0.003344214411667234, 96345)
                ('6971', "Ferris Bueller's Day Off", 0.003343265304428949, 96068)
                ('8644', 'Catch Me If You Can', 0.0033087058502456815, 95126)
                ('6974', 'The Usual Suspects', 0.003262941580608951, 93614)
                ('6206', 'My Big Fat Greek Wedding', 0.003174242919047401, 91301)
                ('1962', '50 First Dates', 0.003173220157758221, 91442)
                ('5926', 'Fight Club', 0.003122575958917768, 89617)
                ('886', 'Ray', 0.003060051247759694, 87971)
                ('5862', 'Memento', 0.0030462798616575346, 87356)
                ('4656', 'Erin Brockovich', 0.003021716527847904, 86939)
                ('7617', 'Dirty Dancing', 0.0029294839351393528, 84465)
                ('5582', 'Star Wars: Episode V: The Empire Strikes Back', 0.0029149732976407254, 83744)
                ('5085', 'Seabiscuit', 0.002914780089884856, 83819)
                ('1798', 'Lethal Weapon', 0.002896439768176203, 83393)
                ('6134', 'Collateral', 0.0028463576625722764, 81875)
                ('1180', 'A Beautiful Mind', 0.0028152734336997585, 80897)
                ('457', 'Kill Bill: Vol. 2', 0.00279658696291656, 80293)
                ('2152', 'What Women Want', 0.002773474034585306, 80015)
                ('8596', 'Seven', 0.0027608841986760152, 79319)
                ('2913', 'Finding Neverland', 0.002755713477514582, 79177)
                ('3106', 'Ghost', 0.002753509074922442, 79315)
                ('4996', 'Gone in 60 Seconds', 0.002699963741494877, 77937)
                ('3860', 'Bruce Almighty', 0.0026754913660434294, 77092)
                ('30', "Something's Gotta Give", 0.00266685905276144, 76743)
                ('7745', 'Apollo 13', 0.0026542763600500463, 76329)
                ('5732', 'GoodFellas: Special Edition', 0.002637277749526613, 75669)
                ('1110', 'Secondhand Lions', 0.002606968487336092, 75036)
                ('7193', 'The Princess Bride', 0.0025714417037448886, 73819)
                ('8327', "One Flew Over the Cuckoo's Nest", 0.0025650202389007263, 73522)
                ('1542', 'Sleepless in Seattle', 0.0025165300593689406, 72435)
                ('6797', 'The Breakfast Club', 0.0025024492415102206, 71920)
                ('4577', 'Steel Magnolias', 0.002480346233545999, 71386)
                ('7057', 'Lord of the Rings: The Two Towers: Extended Edition', 0.002465965712357224, 70840)
                ('191', 'X2: X-Men United', 0.0024567447406700273, 70639)
                ('6196', 'The Terminator', 0.0024401359632700162, 70108)
                ('7230', 'The Lord of the Rings: The Fellowship of the Ring: Extended Edition', 0.0024264349424322597, 69702)
                ('8782', 'The Royal Tenenbaums', 0.0024070729954138483, 68968)
                ('8387', 'Minority Report', 0.0023751217651928084, 68275)
                ('1865', 'Eternal Sunshine of the Spotless Mind', 0.002373232457859721, 68043)
                ('175', 'Reservoir Dogs', 0.002337499618068554, 67030)
                ('7233', 'Stand by Me', 0.0023200019453870537, 66650)
                ('6029', 'Amelie', 0.0023045755012812952, 65966)
                ('2122', 'Being John Malkovich', 0.002299961960160396, 65860)
                ('4951', 'Runaway Jury', 0.0022915214763487316, 65983)
                ('4356', 'Road to Perdition', 0.0022714552867136983, 65288)
                ('1470', 'Bend It Like Beckham', 0.002267931249546875, 65091)
                ('6274', 'The Hunt for Red October', 0.002267518083085493, 65199)
                ('5293', 'Patriot Games', 0.002248135882389078, 64740)
                ('2342', 'Super Size Me', 0.0021822331749609095, 62605)
                ('3290', 'The Godfather', 0.0021676855743921397, 62167)
                ('4123', 'Patch Adams', 0.0021598884491469098, 62267)
                ('6099', 'Apocalypse Now', 0.002158062543539752, 61832)
                ('4472', 'Love Actually', 0.002153201165482524, 61878)
                ('4345', 'Bowling for Columbine', 0.002130917492745912, 61044)
                ('1428', 'The Recruit', 0.002126030477181847, 61272)
                ('3282', 'Sideways', 0.0021213287223204817, 60836)
                ('6386', 'Sister Act', 0.002115767179357764, 61033)
                ('8764', 'Happy Gilmore', 0.002111695482675103, 60799)
                ('313', 'Pay It Forward', 0.002108327615432248, 60724)
                ('3151', 'Napoleon Dynamite', 0.0020893439109868923, 59991)
                ('2660', 'When Harry Met Sally', 0.002089078426639373, 60043)
                ('3917', 'Garden State', 0.002084104975472866, 59796)
                ('3427', 'Men in Black II', 0.0020814131150796, 59999)
                ('3605', "The Wizard of Oz: Collector's Edition", 0.0020606920753829527, 59173)
                ('6692', 'Entrapment', 0.0020296692033588506, 58555)
                ('1144', 'Fried Green Tomatoes', 0.00197756438412347, 56857)
                ('7767', 'O Brother', 0.0019718262575046442, 56547)
                ('5401', 'Dodgeball: A True Underdog Story', 0.0019695817392879917, 56691)
                ('8393', 'Ladder 49', 0.0019694623919513064, 56799)
                ('3925', 'The Matrix: Reloaded', 0.0019686887743560915, 56632)
                ('6475', 'Spanglish', 0.001964381679814624, 56534)
                ('6408', 'Good Morning', 0.0019498382604476474, 56054)
                ('8743', 'Ice Age', 0.0019301142372914993, 55547)
                ('798', 'Jaws', 0.0019260781744805144, 55292)
                ('1145', 'The Wedding Planner', 0.0019179053162710958, 55371)
                ('5054', 'Mission: Impossible', 0.0019006370154109108, 54703)
                ('3079', 'The Lion King: Special Edition', 0.0018923157148523235, 54460)
                ('6428', 'To Kill a Mockingbird', 0.001889814849831663, 54160)
                ('8915', 'Terminator 2: Extreme Edition', 0.0018806116180497354, 54031)
                ('4266', 'The Passion of the Christ', 0.001880434702082544, 54144)
  
  
  
  
  ### Movie Rating Prediction using Matrix Factorization using Alternate Least Mean Squarred Error
  * **Observation**:

    * Below table shows the RMSE for different K and for different number of iterations. These are the obseravtion for total users            86059 and  30001 movies. As explained in above preprocessing steps have considered all the users having frequency of greater than        50 and we have taken only 3000 movies with rating 4 and above. Due to the hardware limitation we have to shrink our data. We have observed that because of this shrink in data has resulted in a very a sparse user, movie rating matrix and that has resulted in a high RMSE values. And also for the larger data we need larger value of K with more number of iteration to observe the convergence.

    | K |  Iterations | RMSE |
    |----|------------|-------|
    | 20 | 20 | (0, 0.6366654672670077)(1, 0.6366960124317139)(2, 0.6365642237758912)(3, 0.634831345269006)(4, 0.6346759574882203)(5, 0.6363619977260161)(6, 0.636598221433023)(7, 0.6361974110008833)(8, 0.6302815440326471)(9, 0.63440678350435)
(10, 0.6335288263251854)(11, 0.6337132126843231)(12, 0.6366802349627171)(13, 0.6356737568678673)(14, 0.6367278183402063)(15, 0.636536439472537)(16, 0.6366794818021604)(17, 0.6347322930493754)(18, 0.6312062686330214)(19, 0.6367313102175471)|
    | 50 | 20 | |
    | 40| 30| (0, 0.6366295265500371)(1, 0.6366922250802994)(2, 0.6365599084957302)(3, 0.6348256339315415)(4, 0.6346794710972152)(5, 0.6363616156059488)(6, 0.6365982770787928)(7, 0.6361974349232022)(8, 0.6302815484788988)(9, 0.6344067890576439)(10, 0.6335288016693283(11, 0.6337132215049638)(12, 0.6366802348643507)(13, 0.635673755919491)(14, 0.6367278183444827)(15, 0.6365364396097373)(16, 0.636679481794901)(17, 0.6347322930498198)(18, 0.6312062683062718)(19, 0.6367313102175113)(20, 0.6367330144769615)(21, 0.6367328026285417)(22, 0.6367330549429197)(23, 0.6367005748240043)(24, 0.6367298343838682)(25, 0.6367244373987135)(26, 0.6303682402641523)(27, 0.6346638102874245)(28, 0.6367188102836143)
(29, 0.6302085880930941)|

      
      

 
## Work Division
| Task |  Srishtee Marotkar | Anchal Atlani|
|----|------------|-------|
| Topic Selection and  Finding Data Set | Yes | Yes|
| Understanding of how Page Rank and ALS equations can be used for the recommendation system | Yes | Yes|
| Data Preprocessing for Page Rank | Yes | Yes|
| Creation of Link Graph | Yes | Yes|
| Implementation of personalized page rank| Yes | Yes|
| Execution and Analysis of the Results| Yes | Yes|
| Preprocessing for Matrix Fatcorization| Yes | Yes|
| Creation of rating matrix from the Dataset| Yes | Yes|
| Implementation of Alternate Least Square Algorithm| Yes | Yes|
| Execution and Analysis of the Results| Yes | Yes|
| Final Submission Report| Yes | Yes|


## References
* https://web.stanford.edu/class/msande233/handouts/lecture8.pdf
* https://stanford.edu/~rezab/classes/cme323/S16/projects_reports/parthasarathy_tea.pdf  
* https://github.com/pinnareet/CME323DSGD/blob/master/DSGD.py
* https://blog.insightdatascience.com/explicit-matrix-factorization-als-sgd-and-all-that-jazz-b00e4d9b21ea
* https://pdfs.semanticscholar.org/2962/86ee15e413b39295c78387080e36c822cfd2.pdf 
* https://web.stanford.edu/~ashishg/msande235/spr08_09/Lecture13.pdf
* https://www.nltk.org/



