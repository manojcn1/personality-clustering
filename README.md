# personality-clustering
The understanding of the human psyche and personality provides an advantage to any organization or individual with the
readily available data on hand. On a large scale it can assist any marketing company, business, or electoral candidate in
how they target a specific audience for their advertising or campaigning. On a small scale, a salesman or presenter may
change their approach on how they communicate with their customers and audience.
For our analysis into human psychology, we will analyze a Big Five Personality Test Dataset that contains the responses of
the Big Five Personality Test from around the globe. This includes 50 questions that are scaled from 1 (Strongly Disagree)
to 5 (Strongly Agree) and the estimated country from where it was taken.
The Big Five Personality Test aims to measure and understand individual differences in personality traits, specifically
these five – Openness, Conscientiousness, Extraversion, Agreeableness, and Neuroticism, collectively shortened to
OCEAN. This test can be used for personal and professional development to target areas for improvement, psychological
research to understand human behavior, and employment to evaluate job fit and leadership potential, among many
other applications.
Our primary approach will be through using unsupervised machine learning methods and ensemble techniques to model
and cluster the data into distinct groups and investigate whether there are specific differences between the participants
of different countries.
Group #1 MIS/BUAN 6341.502
2. Exploratory Data Analysis
2.1. Description of Dataset
The data was collected between 2016 and 2018 through an online personality test from a website
(https://ipip.ori.org/newBigFive5broadKey.htm) and involves responses to the questions listed in the next section as well
as other obtainable data from the test taker, IP location to determine the country and response times. We excluded the
response times from this analysis. In total without cleaning the dataset includes 110 columns, and 1.02 million
observations.
2.2. List of Variables
Extroversion
EXT1 I am the life of the party.
EXT2- I don't talk a lot.
EXT3 I feel comfortable around people.
EXT4- I keep in the background.
EXT5 I start conversations.
EXT6- I have little to say.
EXT7 I talk to a lot of different people at parties.
EXT8- I don't like to draw attention to myself.
EXT9 I don't mind being the center of attention.
EXT10- I am quiet around strangers.
Conscientiousness
CSN1 I am always prepared.
CSN2- I leave my belongings around.
CSN3 I pay attention to details.
CSN4- I make a mess of things.
CSN5 I get chores done right away.
CSN6- I often forget to put things back in their proper place.
CSN7 I like order.
CSN8- I shirk my duties.
CSN9 I follow a schedule.
CSN10 I am exacting in my work.
Emotional Stability (Neuroticism)
EST1- I get stressed out easily.
EST2 I am relaxed most of the time.
EST3- I worry about things.
EST4 I seldom feel blue.
EST5- I am easily disturbed.
EST6- I get upset easily.
EST7- I change my mood a lot.
EST8- I have frequent mood swings.
EST9- I get irritated easily.
EST10- I often feel blue.
Openness
OPN1 I have a rich vocabulary.
OPN2- I have difficulty understanding abstract ideas.
OPN3 I have a vivid imagination.
OPN4- I am not interested in abstract ideas.
OPN5 I have excellent ideas.
OPN6- I do not have a good imagination.
OPN7 I am quick to understand things.
OPN8 I use difficult words.
OPN9 I spend time reflecting on things.
OPN10 I am full of ideas.
Agreeableness
AGR1- I feel little concern for others.
AGR2 I am interested in people.
AGR3- I insult people.
AGR4 I sympathize with others' feelings.
AGR5- I am not interested in other people's problems.
AGR6 I have a soft heart.
AGR7- I am not really interested in others.
AGR8 I take time out for others.
AGR9 I feel others' emotions.
AGR10 I make people feel at ease.
There are variables for each of these questions ending in _E which represent the response times for each question.
-This was excluded from this analysis.
Group #1 MIS/BUAN 6341.502
dateload The timestamp when the survey was started.
screenw The width the of user's screen in pixels
screenh The height of the user's screen in pixels
introelapse The time in seconds spent on the landing / intro page
testelapse The time in seconds spent on the page with the survey questions
endelapse The time in seconds spent on the finalization page (where the user was asked to indicate if they had
answered accurately and their answers could be stored and used for research. This dataset only includes
users who answered "Yes" to this question.
IPC The number of records from the user's IP address in the dataset. Should be limited to 1 for cleanliness,
as specified from the dataset author.
lat_appx_lots_of_err approximate latitude of user. Not accurate, as specified from the dataset author.
long_appx_lots_of_err approximate longitude of user. Not accurate, as specified from the dataset author.
Self Generated:
Extroversion Score = 20 + EXT1 - EXT2 + EXT3 - EXT4 + EXT5 - EXT6 + EXT7 - EXT8 + EXT9 - EXT10
Agreeableness Score = 14 - AGR1 + AGR2 - AGR3 + AGR4 - AGR5 + AGR6 - AGR7 + AGR8 + AGR9 + AGR10
Conscientiousness Score = 14 + CSN1 - CSN2 + CSN3 - CSN4 + CSN5 - CSN6 + CSN7 - CSN8 + CSN9 + CSN10
Neuroticism Score = 38 - EST1 + EST2 - EST3 + EST4 - EST5 - EST6 - EST7 - EST8 - EST9 - EST10
Openness Score = 8 + OPN1 - OPN2 + OPN3 - OPN4 + OPN5 - OPN6 + OPN7 + OPN8 + OPN9 + OPN10
These equations come from https://openpsychometrics.org/printable/big-five-personality-test.pdf
This also signifies which questions are negatively associated with the score, and will need to be flipped for this analysis.
Data Cleaning Methods:
• Removed any rows with missing values in the answer columns
• Removed rows with IPC greater than 1 – as suggested by the author for max cleanliness, higher values can
include shared networks or multiple test takers
• Removed any rows with a value of 0 in the answer columns
• Removed rows with NONE in the country column
• Removed rows from countries with fewer than 1000 participants
• Converted the ISO-3166 country codes in the dataset to the ISO-A3 code used by geopandas
• Renamed columns negatively associated to scores by appending a hyphen ‘-‘.
• Removed columns:
o 50 reaction time columns ending in _E as this was not included in this analysis
o IPC as all remaining values are 1
o latitude and longitude columns as these are considered unaccurate
o Unused columns:
▪ screenw, screenh, introelapse, testelapse, endelapse
Resulting cleaned dataset consists of 62 columns and 570k observations
