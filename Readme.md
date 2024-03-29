# 01F JavaScript project #3: Graphql

  
This project is deployed on netlify and on github: 

https://tranquil-genie-b49c6d.netlify.app
https://athenahta2.github.io/My_skills/  

Project APIs:
graphiql: https://learn.01founders.co/graphiql
fetch requests: https://learn.01founders.co/api/graphql-engine/v1/graphql


Project tasks:

step 1: 
In VSC terminal type: curl "https://learn.01founders.co/graphiql/api/graphql-engine/v1/graphql" --data '{"query":"{user{userid login}}"}'

step2: paste above query result into an html file and open it with live server

step3: make a note of all fields from each 01F table:

user table---> : id, login;
object table---> : campus, childrenAttrs, id, name, type, author, objects, progress, reference, results;
progress table---> : campus, createdAt, grade, id, isDone, object, objectId, path, results, updatedAt, user, userId;
result table---> : campus, createdAt, grade, groupId, object, objectId, path, type, updatedAt, user, userId;
transaction table---> : amount, createdAt, isBonus, object, objectId, path, type, user, userId;
step4: decide which data you need in order to build three sections from list below:
    - Basic user identification
    - XP amount
    - level
    - grades
    - audits
    - skills

step4: write some graphql queries to retrieve data.
step5: write svg code to graph data.

# Generate a jason web token (jwt) to gain access to Gitea data
# The Postman approach:

In Postman, create a new collection. Click on the 'Authorization' tab and choose Type: 'Basic Authentication'.
Input your Username and Password and press the 'Send' button to generate a jwt. 
Click on the '</>' symbol and select 'JavaScript-Fetch' in the 'Code snippet' drop-down box to view the corresponding JavaScript fetch function.

Alternatively, in 'Authorization' tab choose 'Type' = 'Bearer Token', 
and paste the above jwt in the text box to the right.
Next, select the 'Body' tab, write your graphql query in the 'QUERY' window, and press the 'Send' button to view the data.
You can also click on the '</>' symbol and select 'JavaScript-Fetch' 'Code snippet' to view the JavaScript fetch function.

# The graphql query:

the API: https://learn.01founders.co/api/graphql-engine/v1/graphql
my graphql query, used for profile data, projects' details,  
XP by project line graph, XP by task pie chart.

{
    user(where: {login: {_eq: "login"}}) {
			...HelenaId
    }
  
 
    progress(
      where: {_and: [{user: {login: {_eq: "login"}}}, {object: {type: {_eq: "project"}}}, {isDone: {_eq: true}}, {grade: {_neq: 0}}]}
      order_by: {updatedAt: desc}
      
    ) {
      ...HelenaProgress
    }
    transaction(
      where: {_and: [{user: {login: {_eq: "login"}}}, {object: {type: {_eq: "project"}}}, {type: {_eq: "xp"}}]}
      order_by: {amount:desc} 
    ) {
      ...HelenaXP
    }
    tasksTypes:  transaction(where: { userId: { _eq: userId }, type: {_like: "%skill%"}}){
        type
        amount
      }
}
 fragment HelenaId on user{
  login
  id
}

fragment HelenaProgress on progress{
    id
    grade
    createdAt
    updatedAt
    object {
    id
    name
    campus
    }
  }
  
  fragment HelenaXP on transaction{
    amount
    createdAt
    object {
          id
          name
        }
  }
