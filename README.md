Download Link: https://assignmentchef.com/product/solved-math551-lab-9
<br>
<strong>Goal: </strong>Use a norm and an inner product to detect similarity between users’ choices and to create a simple recommender system.

<strong>Getting started: </strong>Open a new Matlab script and save it as “m551lab09.m”.

<ul>

 <li>Open a new Matlab script and save it as “m551lab09.m”.</li>

 <li>Download the file “users movies.mat” which contains the arrays movies, users movies, users movies sort, index small, trial user and place it in your working directory.</li>

</ul>

<strong>What you have to submit: </strong>The file m551lab09.m which you will create during the lab session.

<h1>Reminders About the Grading Software</h1>

Remember, the labs are graded with the help of software tools. If you do not follow the instructions, you will lose points. If the instructions ask you to assign a value to a variable, you must <strong>use the variable name given in the instructions</strong>. If the instructions ask you to make a variable named x1 and instead you make a variable named x or X or X1 or any name other than x1, the grading software will not find your variable and you <em>will </em>lose points. Required variables are listed in the lab instructions in a gray box like this:

<h2>Variables: x1, A, q</h2>

At times you will also be asked to answer questions in a comment in your M-file. You must <strong>format your text answers correctly</strong>. Questions that you must answer are shown in gray boxes in the lab. For example, if you see the instruction

Q7: What does the Matlab command lu do? your file must contain a line similar to the following somewhere

% Q7: It computes the LU decomposition of a matrix.

The important points about the formatting are

<ul>

 <li>The answer is on a single line.</li>

 <li>The first characters on the line is the comment character %.</li>

 <li>The answer is labeled in the form Q<em>n</em>:, where <em>n </em>stands for the question number from the gray box.</li>

</ul>

If you do not format your answers correctly, the grading software will not find them and you <em>will </em>lose points.

<h1>Introduction</h1>

Being able to predict users’ choices is a big business. Many internet services are studying your choices and preferences to be able to offer you the products you might like (and correspondingly the products you are more likely to buy). Netflix, Pandora radio, Spotify, Facebook targeted ads, and even online dating websites are all examples of recommender systems. These services are sometimes willing to go to great lengths in order to improve their algorithms. In fact, in 2006, Netflix offered 1 million dollars to the team which would be able to improve their Cinematch algorithm predictions by just 10%. Music Genome Project which is a trademark of Pandora radio is another example of a recommender system. Every song is assigned a rating from 0 to 5 in 450 different categories (“genes”). Thus, any song is represented by a vector with 450 components. Mathematical comparison of these vectors allows then to suggest the next song to a user.

Observe that data about movies, consumer products, and etc, can be often arranged in a form of a vector. These vector representations can be then used in different algorithms to compare items and predict similarity. In this project, we will use linear algebra to establish similarities in tastes between different users. The main idea is that if we know the current ratings from a certain user, then comparing it with the ratings of other users in database, we can find users with similar tastes. Finally, we can look at the items highly rated by those users and suggest these items to the current user. In this lab, we will look at the MovieLens database which contains around 1 million ratings of 3952 movies by 6040 users. We will create a very simple recommender system by finding similarities between users’ tastes. Our techniques will be based on Euclidean distance (norm), scalar product, and finding correlation coefficients (or angles between the data).

<h1>Tasks</h1>

<ol>

 <li>Load the arrays movies, users movies, users movies sort, index small, trial user from the file users movies.mat using the load The matrix users movies should be a 6040 × 3952 matrix containing integer values between 0 and 5 with 1 meaning “strongly dislike” and 5 meaning “strongly like”. The 0 in the matrix means that the user did not rate the movie. The array movies contains all the titles of the movies. The matrix users movies sort contains an extract from the matrix users movies.mat with only rating for the 20 popular movies selected. The indexes of these popular movies are recorded in the array index small. Finally, ratings of these popular movies by yet another user (not the one contained in the database) are given by the vector trial user. It is suggested to view all the variables and their dimensions by using the “Workspace” window of Matlab environment:</li>

</ol>

%% Load the data clear; load(’users_movies.mat’,’movies’,’users_movies’,’users_movies_sort’,… ’index_small’,’trial_user’);

[m,n]=size(users_movies);

<table width="520">

 <tbody>

  <tr>

   <td width="520">Variables: movies, users movies, users movies sort, index small, trial user, m, n</td>

  </tr>

 </tbody>

</table>

<ol start="2">

 <li>Print the titles of the popular movies at which ratings we will be looking by using the following code:</li>

</ol>

fprintf(’Rating is based on movies:
’) for j=1:length(index_small)

fprintf(’%s 
’,movies{index_small(j)})

end; fprintf(’
’)

Observe that the movie titles are called from the cell array movies (notice the curly parentheses) by using an intermediate array index small.

<ol start="3">

 <li>Now let us select the users we will compare the trial user Here, we want to select the people who rated all of the 20 movies under consideration. This means that there should not be zeros in corresponding rows of the matrix users movies sort. This can be accomplished by the following code:</li>

</ol>

%% Select the users to compare to

[m1,n1]=size(users_movies_sort); ratings=[]; for j=1:m1 if prod(users_movies_sort(j,:))~=0

ratings=[ratings; users_movies_sort(j,:)];

end;

end;

Observe the use of the command prod to compute the product of the elements of the row of the array users movies sort. Now, the array ratings contains all the users which have rated all the popular movies from the array index small. In general, it may happen that this array is empty or too small to create any meaningful comparison – we won’t consider these cases in this project.

<table width="280">

 <tbody>

  <tr>

   <td width="175">Variables: ratings, m1, n1</td>

   <td width="104"></td>

  </tr>

  <tr>

   <td colspan="2" width="280">Q1: What does the command ratings=[] do?</td>

  </tr>

 </tbody>

</table>

<ol start="4">

 <li>Next, we can look at the similarity metric based on the Euclidean distance. The idea here is that we treat the array trial user and the rows of the array ratings as vectors in 20-dimensional real space R<sup>20</sup>. Assume that all the vectors have the origin as the beginning point. We can find the distance between the end points of the vector trial user and each of the vectors ratings(j,:). In other words, we are looking for the user with the closest ratings to the trial user. This can be accomplished by the following code:</li>

</ol>

%% Find the Euclidean distances [m2,n2]=size(ratings); for i=1:m2 eucl(i)=norm(ratings(i,:)-trial_user);

end;

The vector eucl contains all the Euclidean distances between trial user and the rows of the matrix ratings.

<h2>Variables: eucl</h2>

<ol start="5">

 <li>Now let us select the user with the smallest Euclidean distance. Instead of using the usual function min we will use a slightly more complicated approach. Let us sort the elements of the vector eucl by using the function sort. The advantage of this is that it allows us to find the second closest user, the third closest user, etc. There may be only a small difference between several closest users and we might want to use their data as well.</li>

</ol>

[MinDist,DistIndex]=sort(eucl,’ascend’); closest_user_Dist=DistIndex(1)

<table width="313">

 <tbody>

  <tr>

   <td width="313">Variables: MinDist, DistIndex, closest user Dist</td>

  </tr>

 </tbody>

</table>

<ol start="6">

 <li>The similarity metric above is one of the simplest ones which can be used to compare two objects. However, when it comes to user ratings it has certain disadvantages. For instance, what if the users have similar tastes, but one of them consistently judges movies harsher than the other one? The metric above would rate those two users as dissimilar since the Euclidean distance between vectors of their opinions might be pretty large. To rectify this problem, we can look at a different similarity metric known in statistics as the Pearson correlation coefficient which can be defined as</li>

</ol>

<em>,</em>

where <strong>x </strong>= (<em>x</em><sub>1</sub><em>,x</em><sub>2</sub><em>,…,x<sub>N</sub></em>)<em>,</em><strong>y </strong>= (<em>y</em><sub>1</sub><em>,y</em><sub>2</sub><em>,…,y<sub>N</sub></em>) are two vectors with <em>N </em>components, and <em>x </em>=  are the average (mean) values of the vectors <strong>x </strong>and <strong>y</strong>. If we look at

the formula for <em>r</em>(<strong>x</strong><em>,</em><strong>y</strong>), we can see that it accounts for the average user opinion <em>x,y</em>, and also for how harshly/exuberantly they tend to judge their movies (magnitude of the vectors <strong>x</strong>−<strong>x </strong>and <strong>y</strong>−<strong>y</strong>). Geometrically, the <em>r</em>(<strong>x</strong><em>,</em><strong>y</strong>) corresponds to the cosine angle between the vectors <strong>x </strong>− <strong>x </strong>and <strong>y </strong>− <strong>y </strong>(the closer this angle is to zero, the more similar are the opinions of two people). To compute the Pearson correlation coefficient, let us centralize the columns of the matrix ratings and the vector trial user first:

ratings_cent=ratings-mean(ratings,2)*ones(1,n2); trial_user_cent=trial_user-mean(trial_user);

<table width="228">

 <tbody>

  <tr>

   <td width="228">Variables: ratings cent, trial user</td>

  </tr>

 </tbody>

</table>

<ol start="7">

 <li>Next, use the for … end loop to compute the Pearson correlation coefficients between the rows of the matrix ratings and the vector trial user. Save the result as a vector pearson.</li>

</ol>

<h2>Variables: pearson</h2>

<ol start="8">

 <li>Finally, observe that the value <em>r</em>(<strong>x</strong><em>,</em><strong>y</strong>) belongs to the interval (−1<em>,</em>1). The closer the coefficient is to 1, the more similar the tastes of the users are. Finally, let us sort the vector pearson as before using the sort Save the results of this function as [MaxPearson<em>,</em>PearsonIndex] and find the maximal correlation coefficient which will be the first element in the matrix MaxPearson. Save this element as closest user Pearson.</li>

</ol>

<table width="376">

 <tbody>

  <tr>

   <td width="376">Variables: MaxPearson, PearsonIndex, closest user Pearson</td>

  </tr>

 </tbody>

</table>

<ol start="9">

 <li>Compare the elements of the vectors DistIndex, PearsonIndex.</li>

</ol>

<table width="481">

 <tbody>

  <tr>

   <td width="481">Q2: Are the variables closest user Pearson and closest user Dist the same?</td>

  </tr>

 </tbody>

</table>


