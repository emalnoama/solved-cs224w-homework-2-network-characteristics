Download Link: https://assignmentchef.com/product/solved-cs224w-homework-2-network-characteristics
<br>
One of the goals of network analysis is to find mathematical models that characterize real-world networks and that can then be used to generate new networks with similar properties. In this problem, we will explore two famous models—Erd˝os-R´enyi and Small World—and compare them to real-world data from an academic collaboration network. Note that in this problem all networks are <em>undirected</em>. You may use the starter code in hw1-q1-starter.py for this problem.

<ul>

 <li><em>Erd˝os-R´enyi Random graph (G</em>(<em>n,m</em>) <em>random network): </em>Generate a random instance of this model by using <em>n </em>= 5242 nodes and picking <em>m </em>= 14484 edges at random. Write code to construct instances of this model, i.e., do not call a SNAP function.</li>

 <li><em>Small-World Random Network: </em>Generate an instance from this model as follows: begin with <em>n </em>= 5242 nodes arranged as a ring, i.e., imagine the nodes form a circle and each node is connected to its two direct neighbors (e.g., node 399 is connected to nodes 398 and 400), giving us 5242 edges. Next, connect each node to the neighbors of its neighbors (e.g., node 399 is also connected to nodes 397 and 401). This gives us another 5242 edges. Finally, randomly select 4000 pairs of nodes not yet connected and add an edge between them. In total, this will make <em>m </em>= 5242·2+4000 = 14484 edges. Write code to construct instances of this model, i.e., do not call a SNAP function.</li>

 <li><em>Real-World Collaboration Network: </em>Download this undirected network from <a href="http://snap.stanford.edu/data/ca-GrQc.txt.gz">http://snap. </a><a href="http://snap.stanford.edu/data/ca-GrQc.txt.gz">edu/data/ca-GrQc.txt.gz</a><a href="http://snap.stanford.edu/data/ca-GrQc.txt.gz">.</a> Nodes in this network represent authors of research papers on the arXiv in the General Relativity and Quantum Cosmology section. There is an edge between two authors if they have co-authored at least one paper together. Note that some edges may appear twice in the data, once for each direction. Ignoring repeats and self-edges, there are 5242 nodes and 14484 edges. (Note: Repeats are automatically ignored when loading an (un)directed graph with SNAP’s LoadEdgeList function).</li>

</ul>

<h3>1.1       Degree Distribution [</h3>

Generate a random graph from both the Erd˝os-R´enyi (i.e., <em>G</em>(<em>n,m</em>)) and Small-World models and read in the collaboration network. Delete all of the self-edges in the collaboration network (there should be 14,484 total edges remaining).

Plot the degree distribution of all three networks <em>in the same plot </em>on a log-log scale. In other words, generate a plot with the horizontal axis representing node degrees and the vertical axis representing the proportion of nodes with a given degree (by “log-log scale” we mean that both the horizontal and vertical axis must be in logarithmic scale). In one to two sentences, describe one key difference between the degree distribution of the collaboration network and the degree distributions of the random graph models.

<h3>1.2        Clustering Coefficient</h3>

Recall that the local clustering coefficient for a node <em>v<sub>i </sub></em>was defined in class as

0 otherwise<em>,</em>

where <em>k<sub>i </sub></em>is the degree of node <em>v<sub>i </sub></em>and <em>e<sub>i </sub></em>is the number of edges between the neighbors of <em>v<sub>i</sub></em>. The <em>average clustering coefficient </em>is defined as

Compute and report the average clustering coefficient of the three networks. For this question, write your own implementation to compute the clustering coefficient, instead of using a built-in SNAP function.

Which network has the largest clustering coefficient? In one to two sentences, explain. Think about the underlying process that generated the network.

<h2>2            Structural Roles: Rolx and ReFex</h2>

In this problem, we will explore the structural role extraction algorithm Rolx and its recursive feature extraction method ReFex. As part of this exploration, we will work with a dataset representing a scientist co-authorship network, which can be dowloaded at <a href="http://www-personal.umich.edu/~mejn/netdata/netscience.zip">http://www-personal. </a><a href="http://www-personal.umich.edu/~mejn/netdata/netscience.zip">umich.edu/</a><a href="http://www-personal.umich.edu/~mejn/netdata/netscience.zip">~</a><a href="http://www-personal.umich.edu/~mejn/netdata/netscience.zip">mejn/netdata/netscience.zip</a><a href="http://www-personal.umich.edu/~mejn/netdata/netscience.zip">.</a> <a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a> Although the graph is weighted, for simplicity we treat it as <strong>undirected and unweighted </strong>in this problem.

Feature extraction consists of two steps; we first extract basic local features from every node, and we subsequently aggregate them along graph edges so that global features are also obtained. Collectively, feature extraction constructs a matrix <em>V </em>∈ R<em><sup>n</sup></em><sup>×<em>f </em></sup>where for each of the <em>n </em>nodes we have <em>f </em>features to cover local and global information. Rolx extracts node roles from that matrix.

<h3>2.1       Basic Features</h3>

We begin by loading the graph <em>G </em>provided in the bundle and computing three basic features for the nodes. For each node <em>v</em>, we choose 3 basic local features (in this order):

<ol>

 <li>the degree of <em>v</em>, i.e., deg(<em>v</em>);</li>

 <li>the number of edges in the egonet of <em>v</em>, where egonet of <em>v </em>is defined as the subgraph of <em>G </em>induced by <em>v </em>and its neighborhood;</li>

 <li>the number of edges that connect the egonet of <em>v </em>and the rest of the graph, i.e., the number of edges that enter or leave the egonet of <em>v</em>.</li>

</ol>

We use <em>V</em>˜<em><sub>u </sub></em>to represent the vector of the basic features of node <em>u</em>. For any pair of nodes <em>u </em>and <em>v</em>, we can use cosine similarity to measure how similar two nodes are according to their feature vectors <em>x </em>and <em>y</em>:

Sim(;

Also, when ||<em>x</em>||<sub>2 </sub>= 0 or ||<em>y</em>||<sub>2 </sub>= 0, we defined Sim(<em>x,y</em>) = 0<em>.</em>

Compute the basic feature vector for the node with ID 9, and report the top 5 nodes that are most similar to node 9 (excluding node 9). As a sanity check, no element in <em>V</em>˜<sub>9 </sub>is larger than 10.

<h3>2.2       Recursive Features</h3>

In this next step, we recursively generate some more features. We use <em>mean </em>and <em>sum </em>as aggregation functions.

Initially, we have a feature vector <em>V</em>˜<em><sub>u </sub></em>∈ R<sup>3 </sup>for every node <em>u</em>. In the first iteration, we concatenate the mean of all <em>u</em>’s neighbors’ feature vectors to <em>V</em>˜<em><sub>u</sub></em>, and do the same for <em>sum</em>, i.e.,

<em>,</em>

where <em>N</em>(<em>u</em>) is the set of <em>u</em>’s neighbors in the graph. If <em>N</em>(<em>u</em>) = ∅, set the mean and sum to 0.

After <em>K </em>iterations, we obtain the overall feature matrix <em>V </em>= <em>V</em><sup>˜(<em>K</em>) </sup>∈ R<sup>3</sup><em><sup>K</sup></em><sup>+1</sup>.

For this exercise, run <em>K </em>= 2 iterations, and report the top 5 nodes that are most similar to node 9 (excluding node 9). If there are ties, e.g. 4th, 5th, and 6th have the same similarity, report any of them to fill up the top-5 ranking. As a sanity check, the similarities between the reported nodes and node 9 are all greater than 0.9. [5 points]

Compare your obtained top 5 nodes with previous results from 2.1. In particular, are there common nodes? Are there different nodes? In one sentence, why would this change? [3 points]

<h3>2.3       Role Discovery</h3>

In this part, we explore more about the graph according to the recursive feature vectors of nodes and node similarity.

<ul>

 <li>Produce a 20-bin histogram to show the distribution of cosine similarity between node 9 andany other node in the graph (according to their recursive feature vectors). Note here that the x-axis is cosine similarity with node 9, and the y-axis is the number of nodes. [3 points]</li>

</ul>

According to the histogram, can you spot some <em>groups </em>/ <em>roles</em>? How many can you spot? (<em>Clue: look for the spikes!</em>) [2 points]

<ul>

 <li>For these groups / roles in the cosine similarity histogram, take one node <em>u </em>from each group to examine the feature vector, and draw the subgraph of the node based on its feature vector. You can draw the subgraph by hand, or you can use libraries such as networkx or graphviz.</li>

</ul>

For these drawings, you should use the local features for <em>u</em>, and pay attention to the features aggregated from its 1-hop neighbors, but feel free to ignore further features if they are difficult to incorporate. Also, you should not draw nodes that are more than 3-hops away from <em>u</em>. [6 points]

Briefly argue how different structural roles are captured in 1-2 sentences. [1 point]

Community detection using the Louvain algorithm

<strong>Note: </strong>For this question, assume all graphs are undirected and weighted.

Communities are a fundamental aspect of several networks. However, it is often not an easy task to come up with an “optimal” grouping of nodes into communities. Through this problem, we will explore some properties of the Louvain algorithm for community detection (so named because the authors were all affiliated with the University of Louvain in Belgium at some point). The original paper from Blondel et al. is available here: <a href="https://arxiv.org/pdf/0803.0476.pdf">https://arxiv.org/pdf/0803.0476.pdf</a><a href="https://arxiv.org/pdf/0803.0476.pdf">.</a> If you are stuck on this question at any point please refer to the paper; there is a good chance that you will find what you seek there.

We will first explore the idea of modularity. The modularity of a weighted graph is a measure that compares the density of edges within a community to the density of edges between communities. Formally, we define the modularity <em>Q </em>for a given graph as follows:

(1)

Here 2<em>m </em>= Σ<em>A<sub>ij </sub></em>is the sum of all entries in the adjacency matrix, <em>A<sub>ij </sub></em>represents the (<em>i,j</em>)<em><sup>th </sup></em>entry of the adjacency matrix, <em>d<sub>i </sub></em>represents the degree of node <em>i</em>, <em>δ</em>(<em>c<sub>i</sub>,c<sub>j</sub></em>) is 1 when <em>i </em>and <em>j </em>are in the same community (<em>c<sub>i </sub></em>= <em>c<sub>j</sub></em>) and 0 otherwise. Note that we treat communities as disjoint. In other words, a given node from a graph can only belong to one community in that graph.

The modularity of a graph lies in the range [−1<em>,</em>1]. Maximizing the modularity of a given graph is a computationally hard problem, so we try different heuristics for this purporse. One such heuristic is the <strong>Louvain algorithm</strong>. This algorithm outperforms many similar algorithms in terms of both speed as well as maximum modularity obtained. We will run a few steps of the algorithm on a couple of example networks to gain some insights about its behavior and properties.

Each pass of the algorithm has two phases, and proceeds as follows:

<ul>

 <li><strong>Phase 1 (Modularity Optimization): </strong>Start with each node in its own community.</li>

 <li>For each node <em>i</em>, go over all the neighbors <em>j </em>of <em>i</em>. Calculate the change in modularity when <em>i </em>is moved from its present community to <em>j</em>‘s community. Find the neighbor <em>j<sub>m </sub></em>for which this process causes the greatest increase in modularity, and assign <em>i </em>to <em>j<sub>m</sub></em>‘s community (break ties arbitrarily). If there is no positive increase in modularity during this process, keep <em>i </em>in its original community.</li>

 <li>Repeat the above process for each node (going over nodes multiple times if required) until there is no further maximization possible (that is, each node remains in its community). This is the end of Phase 1.</li>

 <li><strong>Phase 2 (Community Aggregation): </strong>Once Phase 1 is done, we contract the original graph <em>G </em>to a new graph <em>H</em>. Each community found in <em>G </em>after Phase 1 becomes a node in <em>H</em>. The weights of edges in between 2 nodes in <em>H </em>are given by the sums of the weights between the respective 2 communities in <em>G</em>. The weights of edges within a community in <em>G </em>sum up to form a self-edge of the same weight in <em>H </em>(be careful while calculating self-edge weights, note that you will have to go once over each original node within the community in <em>G</em>). This is the end of Phase 2. Phase 1 and Phase 2 together make up a single pass of the algorithm.</li>

 <li>Repeat Phase 1 and Phase 2 again on <em>H </em>and keep proceeding this way until no further improvement is seen (you will reach a point where each node in the graph stays in its original community to maximize modularity). The final modularity value is a heuristic for the maximum modularity of the graph.</li>

</ul>

Figure 1: Example from Blondel et al. showing the two phases of the Louvain algorithm. Note how weights for self-edges are assigned in the Community Aggregation phase.

An example taken from Blondel et al. (Figure 1) illustrates the two phases of the algorithm. Note how weights for self-edges are assigned in the Community Aggregation phase – we will need to use this later. The weight of the self-edge formed by merging all nodes in a community <em>K </em>would be given by Σ<em><sub>i</sub></em><sub>∈<em>K</em></sub>Σ<em><sub>j</sub></em><sub>∈<em>K</em></sub><em>A<sub>ij </sub></em>– you can verify this for yourself by checking in Figure 1 that node 11 and 13 have an edge of weight 1 between them, but the corresponding self-edge has a weight of 2.

<h3>3.1             Modularity gain when an isolated node moves into a community</h3>

Consider a node <em>i </em>that is in a community all by itself. Let <em>C </em>represent an existing community in the graph. Node <em>i </em>feels lonely and decides to move into the community <em>C</em>, we will inspect the change in modularity when this happens.

This situation can be modeled by a graph (Figure 2) with <em>C </em>being represented by a single node. <em>C </em>has a self-edge of weight Σ<em><sub>in</sub></em>. There is an edge between <em>i </em>and <em>C </em>of weight <em>k<sub>i,in</sub>/</em>2 (to stay consistent with the notation of the paper). The total degree of <em>C </em>is Σ<em><sub>tot </sub></em>and the degree of <em>i </em>is <em>k<sub>i</sub></em>. As always, 2<em>m </em>= Σ<em>A<sub>ij </sub></em>is the sum of all entries in the adjacency matrix. To begin with, <em>C </em>and <em>i </em>are in separate communities (colored green and red respectively). Prove that the modularity gain seen when <em>i </em>merges with <em>C </em>(i.e., the change in modularity after they merge into one community) is given by:

(2)

<strong>Hint: </strong>Using the community aggregation step of the Louvain method may make computation easier.

In practice, this result is used while running the Louvain algorithm (along with a similar related result) to make incremental modularity computations much faster.

Figure 2: Before merging, <em>i </em>is an isolated node and <em>C </em>represents an existing community. The rest of the graph can be treated as a single node for this problem.

<h3>3.2        Louvain algorithm on a 16 node network</h3>

Consider the graph <em>G </em>(Figure 3), with 4 cliques of 4 nodes each arranged in a ring. Assume all the edges have same weight value 1. There exists exactly one edge between any two adjacent cliques. We will manually inspect the results of the Louvain algorithm on this network. The first phase of modularity optimization detects each clique as a single community (giving 4 communities in all). After the community aggregation phase, the new network <em>H </em>will have 4 nodes.

Figure 3: <em>G </em>is a graph with 16 nodes (4 cliques with 4 nodes per clique).

<ul>

 <li>What is the weight of any edge between two distinct nodes in <em>H</em>? [1 point]</li>

 <li>What is the weight of any self-edge in <em>H</em>? [2 point]</li>

 <li>What is the modularity of <em>H </em>(with each node in its own community)? The easiest way to calculate modularity would be to directly apply the definition from 1 to <em>H </em>(also holds for upcoming questions of this type). [2 points]</li>

</ul>

Spoiler alert: In this network, this is the maximum modularity and the algorithm will terminate here. However, assume that we wanted to contract the graph further into a two node network (call it <em>J</em>) by grouping two adjacent nodes in <em>H </em>into one community and the other two adjacent nodes into another community, and then aggregating (following the same rules of the community aggregation phase).

<ul>

 <li>What is the weight of any edge between two distinct nodes in <em>J</em>? [1 point]</li>

 <li>What is the weight of any self-edge in <em>J</em>? [2 point]</li>

 <li>What is the modularity of <em>J </em>(with each node in its own community)? [2 points] As expected, the modularity of <em>J </em>is less than the modularity of <em>H</em>.</li>

</ul>

<h3>3.3        Louvain algorithm on a 128 node network</h3>

Now consider a larger version of the same network, with 32 cliques of 4 nodes each (arranged in a ring as earlier); call this network <em>G<sub>big</sub></em>. Again, assume all the edges have same weight value 1, and there exists exactly one edge between any two adjacent cliques. The first phase of modularity optimization, as expected, detects each clique as a single community. After aggregation, this forms a new network <em>H<sub>big </sub></em>with 32 nodes.

<ul>

 <li>What is the weight of any edge between two distinct nodes in <em>H<sub>big</sub></em>? [1 point]</li>

 <li>What is the weight of any self-edge in <em>H<sub>big</sub></em>? [2 point]</li>

 <li>What is the modularity of <em>H<sub>big </sub></em>(with each node in its own community)? [2 points]</li>

</ul>

After what we saw in the earlier example, we would expect the algorithm to terminate here. However (spoiler alert again), that doesn’t happen and the algorithm proceeds. The next phase of modularity optimization groups <em>H<sub>big </sub></em>into 16 communities with two adjacent nodes from <em>H<sub>big </sub></em>in each community. Call the resultant graph (after community aggregation) <em>J<sub>big</sub></em>.

<ul>

 <li>What is the weight of any edge between two distinct nodes in <em>J<sub>big</sub></em>? [1 point]</li>

 <li>What is the weight of any self-edge in <em>J<sub>big</sub></em>? [2 point]</li>

 <li>What is the modularity of <em>J<sub>big </sub></em>(with each node in its own community)? [2 points]</li>

</ul>

This particular grouping of communities corresponds to the maximum modularity in this network (and not the one with one clique in each community). The community grouping that maximizes the modularity here corresponds to one that would not be considered intuitive based on the structure of the graph.

<h3>3.4        What just happened?</h3>

Explain (in a few lines) why you think the algorithm behaved the way it did for the larger network (you don’t need to prove anything rigorously, a rough argument will do). In other words, what might have caused modularity to be maximized with an “unintuitive” community grouping for the larger network?

What to submit:

Page 7:              • Proof for change in modularity when node <em>i </em>moves to community <em>C</em>.

Page 8:              • Weight of edge between two distinct nodes in <em>H</em>.




<table width="0">

 <tbody>

  <tr>

   <td width="79"> </td>

   <td width="460">•    Weight of self-edge in <em>H</em>.•    Modularity of <em>H </em>with each node in its own community.•    Weight of edge between two distinct nodes in <em>J</em>.•    Weight of self-edge in <em>J</em>.•    Modularity of <em>J </em>with each node in its own community.</td>

  </tr>

  <tr>

   <td width="79">Page 9:</td>

   <td width="460">• The same as above, except for <em>H<sub>big </sub></em>and <em>J<sub>big</sub></em>.</td>

  </tr>

  <tr>

   <td width="79">Page 10:</td>

   <td width="460">• Explanation of algorithm behavior in the larger network (a few lines).</td>

  </tr>

 </tbody>

</table>







<h2>4         Spectral clustering</h2>

This question derives a spectral clustering algorithm that we then use to analyze a real-world dataset. These algorithms use eigenvectors of matrices associated with the graph. You may find this handout https://bit.ly/2l0dXCL on graph clustering to be useful for additional background information.

Let’s first discuss some notation:

<ul>

 <li>Let <em>G </em>= (<em>V,E</em>) be a simple (that is, no self- or multi- edges) undirected, connected graph with <em>n </em>= |<em>V </em>| and <em>m </em>= |<em>E</em>|.</li>

 <li><em>A </em>is the adjacency matrix of the graph <em>G</em>, i.e., <em>A<sub>ij </sub></em>is equal to 1 if (<em>i,j</em>) ∈ <em>E </em>and equal to 0 otherwise.</li>

 <li><em>D </em>is the diagonal matrix of degrees: <em>D<sub>ii </sub></em>= <sup>P</sup><em><sub>j </sub>A<sub>ij </sub></em>= the degree of node <em>i</em>.</li>

 <li>We define the <em>graph Laplacian </em>of <em>G </em>by <em>L </em>= <em>D </em>− <em>A</em>.</li>

</ul>

For a set of nodes <em>S </em>⊂ <em>V </em>, we will measure the quality of <em>S </em>as a cluster with a “cut” value and a “volume” value. We define the cut of the set <em>S </em>to be the number of edges that have one end point in <em>S </em>and one end point in the complement set <em>S</em>¯ = <em>V </em><em>S</em>:

cut(<em>S</em>) = <sup>X </sup><em>A<sub>ij</sub>.</em>

<em>i</em>∈<em>S,j</em>∈<em>S</em>¯

Note that the cut is symmetric in the sense that cut(<em>S</em>) = cut(<em>S</em>¯). The <em>volume </em>of <em>S </em>is simply the sum of degrees of nodes in <em>S</em>:

vol(<em>S</em>) = <sup>X</sup><em>d<sub>i</sub>,</em>

<em>i</em>∈<em>S</em>

where <em>d<sub>i </sub></em>is the degree of node <em>i</em>.

<h3>4.1     A Spectral Algorithm for Normalized Cut Minimization: Foundations [10 points]</h3>

We will try to find a set <em>S </em>with a small normalized cut value:

cut(<em>S</em>)       cut(<em>S</em>¯)

NCUT(<em>S</em>) =      +      (3) vol(<em>S</em>)           vol(<em>S</em>¯)

Intuitively, a set <em>S </em>with a small normalized cut value must have few edges connecting to the rest of the graph (making the numerators small) as well as some balance in the size of the clusters (making the denominators large).

Define the assignment vector <em>x </em>for some set of nodes <em>S </em>such that

(4)

Prove the properties below.

<em>Note: </em>There are many ways to prove the following properties, and we provide some hints for one of the ways. You do not necessarily need to use the provided hints for your proof.

<ul>

 <li><em>L </em>= <sup>P</sup><sub>(<em>i,j</em>)∈<em>E</em></sub>(<em>e<sub>i </sub></em>− <em>e<sub>j</sub></em>)(<em>e<sub>i </sub></em>− <em>e<sub>j</sub></em>)<em><sup>T </sup></em>, where <em>e<sub>k </sub></em>is an <em>n</em>-dimensional column vector with a 1 at position <em>k </em>and 0’s elsewhere. Note that we aren’t summing over the entire adjacency matrix and only count each edge once.</li>

 <li><em>x<sup>T </sup>Lx </em>= <sup>P</sup><sub>(<em>i,j</em>)</sub>∈<em><sub>E</sub></em>(<em>x<sub>i </sub></em>− <em>x<sub>j</sub></em>)<sup>2</sup>. <em>Hint: Apply the result from part (i).</em></li>

 <li><em>x<sup>T </sup>Lx </em>= <em>c</em>NCUT(<em>S</em>) for some constant <em>c </em>(in terms of the problem parameters). <em>Hint: Rewrite the sum in terms of S and S</em>¯<em>.</em></li>

 <li><em>x<sup>T </sup>De </em>= 0, where <em>e </em>is the vector of all ones.</li>

 <li><em>x<sup>T </sup>Dx </em>= 2<em>m</em>.</li>

</ul>

<h3>4.2           Normalized Cut Minimization: Solving for the Minimizer [8 points]</h3>

Since <em>x<sup>T </sup>Dx </em>is just a constant (2<em>m</em>), we can formulate the normalized cut minimization problem in the following way:

<table width="0">

 <tbody>

  <tr>

   <td width="77">minimize <em>S</em>⊂<em>V, x</em>∈R<em><sup>n </sup></em>subject to</td>

   <td width="393"><em>x<sup>T </sup>Dx x<sup>T </sup>De </em>= 0<em>, x<sup>T </sup>Dx </em>= 2<em>m, x </em>as in Equation 4</td>

   <td width="20">(5)</td>

  </tr>

 </tbody>

</table>

<em>x<sup>T </sup>Lx</em>

The constraint that <em>x </em>takes the form of Equation 4 makes the optimization problem NP-hard. We will instead use the “relax and round” technique where we relax the problem to make the optimization problem tractable and then round the relaxed solution back to a feasible point for the original problem. Our relaxed problem will eliminate the constraint that <em>x </em>take the form of Equation 4 which leads to the following relaxed problem:

minimize

<em>x</em>∈R<em><sup>n</sup></em>(6) subject to <em>x<sup>T </sup>De </em>= 0<em>, x<sup>T </sup>Dx </em>= 2<em>m</em>

Show that the minimizer of Equation 6 is <em>D</em><sup>−1<em>/</em>2</sup>v, where <em>v </em>is the eigenvector corresponding to the second smallest eigenvalue of the <em>normalized graph Laplacian L</em><sup>˜ </sup>= <em>D</em><sup>−1<em>/</em>2</sup><em>LD</em><sup>−1<em>/</em>2</sup>. Finally, to round the solution back to a feasible point in the original problem, we can take the indices of all positive entries of the eigenvector to be the set <em>S </em>and the indices of all negative entries to be <em>S</em>¯.

<em>Hint 1: Make the substitution z </em>= <em>D</em><sup>1<em>/</em>2</sup><em>x.</em>

<em>Hint 2: Note that e is the eigenvector corresponding to the smallest eigenvalue of L.</em>

<em>Hint 3: The normalized graph Laplacian L</em>˜ <em>is symmetric, so its eigenvectors are orthonormal and form a basis for </em>R<em><sup>n</sup>. This means we can write any vector x as a linear combination of orthonormal eigenvectors of L</em>˜<em>.</em>

<h3>4.3          Relating Modularity to Cuts and Volumes</h3>

In Problem 3, we presented the modularity of a graph clustering in the context of the Louvain Algorithm. Modularity actually relates to cuts and volumes as well. Let’s consider a partitioning of our graph <em>A </em>into 2 clusters, and let <em>y </em>∈ {1<em>,</em>−1}<em><sup>n </sup></em>be an assignment vector for a set <em>S</em>:

<table width="0">

 <tbody>

  <tr>

   <td width="335">Then, the <em>modularity </em>of the assignment <em>y </em>is</td>

   <td width="269">if <em>i </em>∈ <em>S </em>if <em>i </em>∈ <em>S</em>¯</td>

   <td width="20">(7)</td>

  </tr>

 </tbody>

</table>

<em>.                                                           </em>(8)

Let <em>y </em>be the assignment vector in Equation 7. Prove that

cut(  vol(                                                   (9)

Thus, maximizing modularity is really just minimizing the sum of the cut and the negative product of the partition’s volumes. As a result, we can use spectral algorithms similar to the one derived in parts 1-2 in order to find a clustering that maximizes modularity. While this might provide an intuitively “better” clustering after inspection than the Louvain Algorithm, spectral algorithms are computationally intensive on large graphs, and would only partition the graph into 2 clusters.

<em>Note: </em>You only need to prove the relationship between modularity and cuts; you do <em>not </em>need to derive the actual spectral algorithm.

<h3>What to submit</h3>

Page 11:          • Proofs of the five properties.

Page 12:          • Proof of the minimizer.

Page 13:           • Proof of the relationship between modularity and cuts and volumes.

<a href="#_ftnref1" name="_ftn1">[1]</a> We provide a binary file named <em>hw1-q2.graph </em>for you to directly load into snap by

G = snap.TUNGraph.Load(snap.TFIn(“hw1-q2.graph”)) . You are welcome to either use this file or load from raw data yourself.