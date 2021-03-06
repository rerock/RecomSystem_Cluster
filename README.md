### Goal: To understand recommendation systerms - Clustering 

#### Application 1: Netflix
How to offer customers accurate movie recommendations based on a customers's own perferences and viewing history? 

- <b>Available Data: </b>
  - movie's ranking from all users in the database
  - facts about the movies: actors, directors, genre classifications, year released, etc

- <b>Statistical Techiniques:</b>
  - Collaborative Filtering: only uses other similar user's ratings to make predictions
    - Technique: Clustering
    - Pros: can accuartely suggest complex items without understanding the nature of the items
    - Cons: requires a lot of data about the user to make accurate recommendations 

  - Content Filtering: only uses the information about the movies that users saw before. Not other users inloved 
    - Pros: requires very little data to get started
    - Cons: can be limited in scope (can only recommend similar things to what the user has already liked)
    
  - Hybrid Recommendation System: uses collaborative and content filtering. <br>
For example, first use collaborative filtering approach to determine the similar preferces users group, then do content filtering to find movies that the users group all like, and recommend similar classification movies to the group users. 

## Clustering 
### <b> Pre-Analysis:</b>
- <i> Categories</i> : Movies are categorized as belonging to different genres, each movie may belong to many genres
 - Action, Adventure, Animation, Children's, Comedy, Crime, Documentary, Drama, Fantasy, Film Noir, Horror, Musical, Mystery, Romance, Sci-FI, Thriller, War, Western, (Unknown)
- <b> Raised Question 1 : Is it possible to systematically find groups of movies with similar sets of genres?</b>
  - Answer: Try clustering. 
  
### Usage:
- To segment the data into similar groups
- Not to make predictions, but clustering the data into "similar" groups can improve the predictive methods when building models for each "simliar" group
- Warning: only use if the data set is large, be careful not to overerfit the model

### Algorithms:
- Different algorithms differ in what makes a cluster and how to find them

#### Step 1: Normalize the data points
- Substract the mean of the data and dividing by the standard deviation 

#### Step 2: Distance Between Points 
- Euclidean distance (Most common)

    ![](http://i.imgur.com/4aHtTXF.png)

- Manhattan Distance
  - Sum of absolute values instead of squares
- Maximum Coordiante Distance
  - Only consider measurement for which data points deviate the most 
  
#### Step 3: Distance Between Clusters
- Centroid Distance (Most common) 
  - The distance between centoids of clusters.
    - Centroid is the point that has the average of all data points in each cluster 
    
- Minimum Distance
  - The distance between two points in the clusters that are closest together
  
- Maximum Distance
  - The distance between two points in the clusters that are the farthest
  
#### Algorithm: Hierarchiccal
<ol>
  <li>Each data point respresents a cluster</li>
  <li>Combine two nears clusters (Euclidean, Centroid) </li>
  <li>Continue combining until only one cluster</li>
  <li>Graph the Dendrogram: 
      <ul>
          <li>The data points are listed along the bottom</li>
          <li>The height of the lines represents the distantce between the clusters were when they were combine </li>
      </ul>
  <li>Use the dendrogram to decide how many clusters we want
    <ul>
      <li>Draw a horizontal line across the dendrogram</li>
      <li>The number of vertical lines get crossed is the number of clusters there will be</li>
      <li>The farthest the horizontal line can move up and down without hitting other horizontal lines, the better that choice of the number of cluster is. </li>
      <li>When comparing the number of clusters, we also need to consider how many clusters make sense for the particular application we are working with.</li>
    </ul>
  </li>
 <li>Analyze the picked clusters to see if they are meaningful 
  <ul>
     <li> Look at the basic statistics for each clust: max, min, mean
     <li> Check if the clusters have a feature in common that was not used in the clustering like an outcome variable, which often indicates that the clusters might help improve a predictive model 
</ol>

![](http://i.imgur.com/wttO56U.png)

The above table that was created to better analyze the clusters using the Hierarchiccal algorithm. The highlighted cells have highter than average values in the corresponding clusters. For example, cluster 2 has a high number of action adventure and sci-fi movies. Cluster 1 has a little bit of everything, Cluster 3 has the crime, mystery, thriller movies, etc..

#### How these clusters could be used in a recommendation system? <br>
Let's say Amy like the movie "Men in Black (1997)":
- Find out where is "Men in Black is in" and which cluster does it belong to
  - "Men in Black" went into cluster 2, and that make sense since the cluster 2 is the action, adventure, sci-fi cluster.
- Create a new data set with just the movies from selected cluster
  - The Cluster2 movies are good recommendation to Amy. They are movies like: Apollo 13, Jurassic Park, etc. 

#### Algorithm 2: K-means clustering
- Specify desired number of clusters k
  - the number of cluster k can be selected from previous knowledge or experimenting
  - can use algorithm several times with different random starting points 
- Randomly assign each data point to a cluster
  - can strategically select initial partition of points into clusters if yo have some knowledge of the data 
- Compute cluster centroids
- Re-assign each point to the closest cluster centroid
- Re-compute cluster centroids
- Repeat 4 and 5 until no improvement is made 

### Application 2: cluster-then-predict future stock prices 
- Use clustering to identify clusters of stocks that have similar returns over time
- Use logistic regression to predict whether or not the stocks will have positive future returns

####Goal: 
To predict whether or not the stock return in December will be positive, using the stock returns for the first 11 months of the year.

#### Data: 
StocksCluster.csv, which contains monthly stock returns from the NASDAQ stock exchange. Each observation in the dataset is the monthly returns of a particular company in a particular year. The years included are 2000-2009. The companies are limited to tickers that were listed on the exchange for the entire period 2000-2009, and whose stock price never fell below $1.

#### Caution: 
In cluster-then-predict, the goal is to predict the dependent variable, which is unknown to us at the time of prediction. If the outcome value was used to cluster, although the result might strongly outperforms a non-clustering alternative, it is wrong to use the outcome to determine the clusters. 

When comparing two different clustering algorithms, the clusters are not displayed in a meaningful order, so while there may be a cluster produced by the k-means algorithm that is similar to a cluster produced by the Hierarchical method, it will not necessarily be shown first.

For more details on this application, please check out the posted code.  
