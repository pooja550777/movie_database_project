
# Project Name -
movie_database_project

# Introduction-

This project all about creating movie database with following tables actor, director, movies, movie_cast, rating.


below i used quries to retrive data from movie database table

**SQL queries 

1. List the titles of all movies directed by „Hitchcock‟.
2. Find the movie names where one or more actors acted in two or more movies.
3. List all actors who acted in a movie before 2000 a nd also in a movie after 2015
(use JOIN operation).
4. Find the title of movies and nur of stars for each movie that has at least one
rating and find the highest number of stars that movie received. Sort the result by
movie title.
5. Update rating of all movies directed by „Steven Spielberg‟ to 10.



/* List the titles of all movies directed by „Hitchcock*/
SELECT MOV_TITLE
FROM MOVIES
WHERE DIR_ID = (SELECT DIR_ID
FROM DIRECTOR
WHERE DIR_NAME='HITCHCOCK');

/* Find the movie names where one or more actors acted in two or more movies*/


SELECT MOV_TITLE
FROM MOVIES M,MOVIE_CAST MC
WHERE M.MOV_ID=MC.MOV_ID AND ACT_ID IN (SELECT ACT_ID
FROM MOVIE_CAST GROUP BY ACT_ID
HAVING COUNT(ACT_ID)>1)
GROUP BY MOV_TITLE
HAVING COUNT(*)>1;

/* List all actors who acted in a movie before 2000 a nd also in a movie after 2015*/

SELECT ACT_NAME
FROM ACTOR A
JOIN MOVIE_CAST C
ON A.ACT_ID=C.ACT_ID
JOIN MOVIES M
ON C.MOV_ID=M.MOV_ID
WHERE M.MOV_YEAR NOT BETWEEN 2000 AND 2015;

/*Find the title of movies and nur of stars for each movie that has at least one
rating and find the highest number of stars that movie received. Sort the result by
movie title */

SELECT MOV_TITLE,MAX(REV_STARS)
FROM MOVIES
INNER JOIN RATING USING (MOV_ID)
GROUP BY MOV_TITLE
HAVING MAX(REV_STARS)>0
ORDER BY MOV_TITLE;

/*Update rating of all movies directed by „Steven Spielberg‟ to 10*/


UPDATE RATING
SET REV_STARS=5
WHERE MOV_ID IN (SELECT MOV_ID FROM MOVIES
WHERE DIR_ID IN (SELECT DIR_ID
FROM DIRECTOR
WHERE DIR_NAME='Selvaraghavan'));



