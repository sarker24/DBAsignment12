# DBAsignment12

## run neo4j server on windows:
1.	For running neo4j I have download zip file from the link https://neo4j.com/download-center/#releases
2.	Unzip the zip file 
3.	Go to the cmd prompt with administrator access  and go to the bin folder
4.	Access neo4j.bat file in my case C:\TOOLS\neo4j-community-3.5.4\bin\neo4j.bat
5.	Run ne04j.bat file using install-service command
6.	Use command net start neo4j for starting neo4j database
7.	 For verify run localhost:7474


EX 1:
LOAD CSV WITH HEADERS FROM "https://github.com/sarker24/DBAsignment12/blob/master/some2016UKgeotweets.csv" AS row
WITH row
WHERE NOT row.Latitude IS NULL AND NOT row.Longitude IS NULL
MERGE (t:Tweet
    {
        username:row["User Name"],
        nickname: row.Nickname,
        place: row["Place (as appears on Bio)"],
        latitude: toFloat(row.Latitude),
        longitude: toFloat(row.Longitude),
        mentions: [
            m in filter(m in split(row["Tweet content"]," ") 
            where m starts with "@" and size(m) > 1) | right(m,size(m)-1)
        ],
        content: row["Tweet content"]
    }
);

EX 2:
Use the mentions list of each tweet to create a new set of nodes:
MATCH(t:Tweet) 
WITH t
FOREACH (
    m in t.mentions | 
    MERGE (tu:Tweeters { username:t.username} )
    CREATE (tu)-[:MENTIONS]->(t)
);

Create a relation "Tweeted" between Tweeters and Tweet.
MATCH(t:Tweet) 
WITH t
MERGE (tu:Tweeters { username:t.username} )
CREATE (tu)-[:TWEETED]->(t);



