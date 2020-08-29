---
id: 83171
title: 'Building a Flutter Quiz App using Firebase &#038; Hasura GraphQL Engine &#8211; Part 1'
date: 2020-03-24T12:51:44+00:00
author: Can Taşpınar
layout: post
guid: https://gbot.dev/?p=83171
permalink: /2020/03/24/building-a-flutter-quiz-app-using-firebase-hasura-graphql-engine-part-1/
categories:
  - Uncategorized
---
_This tutorial was initially published on [Hasura blog](https://hasura.io/blog/build-flutter-app-graphql-hasura-serverless-part1/)_

## Introduction {#introduction}

In this blog post we are going to make a quiz app with Flutter powered by Hasura and Firebase. We are going to see how Hasura can work with Firebase services such as Authentication and Cloud Functions to handle user authentication and business logic. Combining these services together will provide us a flexible and scalable architecture.

This will be a 3-part series. In this first part, we are going to deploy Hasura and we will see how to model the relational data with permissions. **Part 2 **will cover how to setup Auth with Firebase and Hasura. **Part 3** will cover how to make the flutter frontend.

## Hasura Deployment on Heroku {#hasura-deployment-on-heroku}

Deploy Hasura less than a minute by using the Heroku button below.

<a href="https://heroku.com/deploy?template=https://github.com/hasura/graphql-engine-heroku" target="_blank" rel="noopener noreferrer"><img src="https://www.herokucdn.com/deploy/button.svg" alt="Hasura on Heroku" /></a>

When it is done, you can access the Hasura Console by opening your Heroku app URL.

##<img class="alignnone size-full wp-image-83175" src="https://gbot.dev/wp-content/uploads/2020/03/1.png" alt="" width="1600" height="934" srcset="https://gbot.dev/wp-content/uploads/2020/03/1.png 1600w, https://gbot.dev/wp-content/uploads/2020/03/1-300x175.png 300w, https://gbot.dev/wp-content/uploads/2020/03/1-1024x598.png 1024w, https://gbot.dev/wp-content/uploads/2020/03/1-768x448.png 768w, https://gbot.dev/wp-content/uploads/2020/03/1-1536x897.png 1536w" sizes="(max-width: 1600px) 100vw, 1600px" />  {#modelling-data}

## Modelling Data

Let’s start with modelling our data first. We are going to define columns, relationships and permissions. Hasura makes it easy to create relationships by tracking foreign keys in tables and suggesting possible relationships. By setting permissions you can restrict access for rows and columns for users by their role.

Head to the **Data > Add Table**

**HINT:** You can use the “Frequently used columns” for these frequently used types:

###<img class="alignnone wp-image-83176 size-full" src="https://gbot.dev/wp-content/uploads/2020/03/2.png" alt="" width="571" height="412" srcset="https://gbot.dev/wp-content/uploads/2020/03/2.png 571w, https://gbot.dev/wp-content/uploads/2020/03/2-300x216.png 300w" sizes="(max-width: 571px) 100vw, 571px" /> 

### 

### Table: users

`id`, `email`, `name`** **columns will be filled by the data coming from Firebase. Notice that `id` here is type of `Text`**.** We are going to talk more about that later on. `score`** **column will be incremented if the user submits the correct answer. `created_at` will be set to now when a new user is inserted to the table. Let’s set `id`** **as the** **primary key.

<img class="alignnone size-full wp-image-83177" src="https://gbot.dev/wp-content/uploads/2020/03/3.png" alt="" width="1343" height="894" srcset="https://gbot.dev/wp-content/uploads/2020/03/3.png 1343w, https://gbot.dev/wp-content/uploads/2020/03/3-300x200.png 300w, https://gbot.dev/wp-content/uploads/2020/03/3-1024x682.png 1024w, https://gbot.dev/wp-content/uploads/2020/03/3-768x511.png 768w" sizes="(max-width: 1343px) 100vw, 1343px" /> 

After creating table head to the **Permissions **tab. For now, there is only one role which is admin and it has full access on every operation below. Enter a new role called “user”. For users table we are only going to give select permissions. A user should be able to see other’s name and score. We are mostly going to deal with following rules when we are adjusting select permissions.

  * **Row select permissions** to restrict access to rows.
  * **Column select permissions** to restrict access for certain columns

We don’t need a custom check for **Row select permissions. **For the **Column select permissions, **check only name and score columns. This way we ensure that users can only see these fields.

<img class="alignnone size-full wp-image-83178" src="https://gbot.dev/wp-content/uploads/2020/03/4.png" alt="" width="1387" height="916" srcset="https://gbot.dev/wp-content/uploads/2020/03/4.png 1387w, https://gbot.dev/wp-content/uploads/2020/03/4-300x198.png 300w, https://gbot.dev/wp-content/uploads/2020/03/4-1024x676.png 1024w, https://gbot.dev/wp-content/uploads/2020/03/4-768x507.png 768w" sizes="(max-width: 1387px) 100vw, 1387px" /> 

**Table: questions**

`id`** **is a field with type UUID and the primary key. Default function `gen_random_uuid()`** **will generate a unique UUID whenever a new object is inserted to the table. **question **is the question title and `created_at` will be used for ordering questions later.

<img class="alignnone size-full wp-image-83179" src="https://gbot.dev/wp-content/uploads/2020/03/5.png" alt="" width="1600" height="799" srcset="https://gbot.dev/wp-content/uploads/2020/03/5.png 1600w, https://gbot.dev/wp-content/uploads/2020/03/5-300x150.png 300w, https://gbot.dev/wp-content/uploads/2020/03/5-1024x511.png 1024w, https://gbot.dev/wp-content/uploads/2020/03/5-768x384.png 768w, https://gbot.dev/wp-content/uploads/2020/03/5-1536x767.png 1536w" sizes="(max-width: 1600px) 100vw, 1600px" /> 

For permissions, every user can make a query to get the questions, so we have the following permission for the questions table.

<img class="alignnone size-full wp-image-83180" src="https://gbot.dev/wp-content/uploads/2020/03/6.png" alt="" width="1385" height="905" srcset="https://gbot.dev/wp-content/uploads/2020/03/6.png 1385w, https://gbot.dev/wp-content/uploads/2020/03/6-300x196.png 300w, https://gbot.dev/wp-content/uploads/2020/03/6-1024x669.png 1024w, https://gbot.dev/wp-content/uploads/2020/03/6-768x502.png 768w" sizes="(max-width: 1385px) 100vw, 1385px" /> 

**Table: question_answers**

`id`** **is again type of UUID and the primary key for the table. `answer` is the question answer. Since we are going to give multiple answer options for a question, we need to know which answer is the correct one. To do that we have the `is_correct` column with a type of Boolean.

<img class="alignnone size-full wp-image-83181" src="https://gbot.dev/wp-content/uploads/2020/03/7.png" alt="" width="1385" height="761" srcset="https://gbot.dev/wp-content/uploads/2020/03/7.png 1385w, https://gbot.dev/wp-content/uploads/2020/03/7-300x165.png 300w, https://gbot.dev/wp-content/uploads/2020/03/7-1024x563.png 1024w, https://gbot.dev/wp-content/uploads/2020/03/7-768x422.png 768w" sizes="(max-width: 1385px) 100vw, 1385px" /> 

`question_id`** **is here to point out which question this answer belongs to. So, it will be a foreign key.

<img class="alignnone wp-image-83182" src="https://gbot.dev/wp-content/uploads/2020/03/8.png" alt="" width="654" height="561" srcset="https://gbot.dev/wp-content/uploads/2020/03/8.png 844w, https://gbot.dev/wp-content/uploads/2020/03/8-300x257.png 300w, https://gbot.dev/wp-content/uploads/2020/03/8-768x659.png 768w" sizes="(max-width: 654px) 100vw, 654px" /> 

Users should be able to make a query to get the possible answers of a question. Permissions for this table is as below.

<img class="alignnone size-full wp-image-83183" src="https://gbot.dev/wp-content/uploads/2020/03/9.png" alt="" width="1384" height="931" srcset="https://gbot.dev/wp-content/uploads/2020/03/9.png 1384w, https://gbot.dev/wp-content/uploads/2020/03/9-300x202.png 300w, https://gbot.dev/wp-content/uploads/2020/03/9-1024x689.png 1024w, https://gbot.dev/wp-content/uploads/2020/03/9-768x517.png 768w" sizes="(max-width: 1384px) 100vw, 1384px" /> 

**Table: user_answers**

`id`** **is type of UUID and primary key. `user_id`, `question_id` and `answer_id`** **are foreign keys to point out to other tables. Notice that `user_id`** **column is type of Text instead of UUID. This is because we defined the `id` field in `users` table as Text. This is because it will be filled by the value coming from Firebase Authentication and Firebase user id is a string.<figure class="kg-card kg-image-card">

<img class="alignnone size-full wp-image-83184" src="https://gbot.dev/wp-content/uploads/2020/03/10.png" alt="" width="1387" height="764" srcset="https://gbot.dev/wp-content/uploads/2020/03/10.png 1387w, https://gbot.dev/wp-content/uploads/2020/03/10-300x165.png 300w, https://gbot.dev/wp-content/uploads/2020/03/10-1024x564.png 1024w, https://gbot.dev/wp-content/uploads/2020/03/10-768x423.png 768w" sizes="(max-width: 1387px) 100vw, 1387px" /><img class="alignnone size-full wp-image-83185" src="https://gbot.dev/wp-content/uploads/2020/03/11.png" alt="" width="950" height="420" srcset="https://gbot.dev/wp-content/uploads/2020/03/11.png 950w, https://gbot.dev/wp-content/uploads/2020/03/11-300x133.png 300w, https://gbot.dev/wp-content/uploads/2020/03/11-768x340.png 768w" sizes="(max-width: 950px) 100vw, 950px" /> </figure> 

For the permission part, we are going to allow users to insert answers, but we need to make sure a user can only insert an answer if the request is coming from the same user. In other words, a user can’t insert a new record on behalf of someone else. This is where **Sessions Variables** comes in. Hit with** custom check **box and give the following rule.<figure>

<img class="alignnone size-full wp-image-83186" src="https://gbot.dev/wp-content/uploads/2020/03/12.png" alt="" width="954" height="452" srcset="https://gbot.dev/wp-content/uploads/2020/03/12.png 954w, https://gbot.dev/wp-content/uploads/2020/03/12-300x142.png 300w, https://gbot.dev/wp-content/uploads/2020/03/12-768x364.png 768w" sizes="(max-width: 954px) 100vw, 954px" /> </figure> <figure class="kg-card kg-image-card">Every request from user that comes to the GraphQL API, will include a `X-Hasura-User-Id` header. We are going to talk more on that in Authentication part. Last thing we need to do is to set to the `user_id` field automatically from the `X-Hasura-User-Id` header. For that we are going to make use of **Column presets. **This allow a column to be filled by a static value or a session variable in our case.</figure> 

<img class="alignnone size-full wp-image-83187" src="https://gbot.dev/wp-content/uploads/2020/03/13.png" alt="" width="1255" height="168" srcset="https://gbot.dev/wp-content/uploads/2020/03/13.png 1255w, https://gbot.dev/wp-content/uploads/2020/03/13-300x40.png 300w, https://gbot.dev/wp-content/uploads/2020/03/13-1024x137.png 1024w, https://gbot.dev/wp-content/uploads/2020/03/13-768x103.png 768w" sizes="(max-width: 1255px) 100vw, 1255px" /> 

At the end insert permissions for user_answers table should look like below.

<img class="alignnone size-full wp-image-83188" src="https://gbot.dev/wp-content/uploads/2020/03/14.png" alt="" width="1384" height="966" srcset="https://gbot.dev/wp-content/uploads/2020/03/14.png 1384w, https://gbot.dev/wp-content/uploads/2020/03/14-300x209.png 300w, https://gbot.dev/wp-content/uploads/2020/03/14-1024x715.png 1024w, https://gbot.dev/wp-content/uploads/2020/03/14-768x536.png 768w" sizes="(max-width: 1384px) 100vw, 1384px" /> 

Now that we defined our tables, it is time to create relationships for GraphQL API. Head to the **Data **tab. Hasura tells you about the Untracked foreign-key relations. Hit **Track All** to expose these relations over GraphQL API.

<img class="alignnone size-full wp-image-83189" src="https://gbot.dev/wp-content/uploads/2020/03/15.png" alt="" width="1338" height="670" srcset="https://gbot.dev/wp-content/uploads/2020/03/15.png 1338w, https://gbot.dev/wp-content/uploads/2020/03/15-300x150.png 300w, https://gbot.dev/wp-content/uploads/2020/03/15-1024x513.png 1024w, https://gbot.dev/wp-content/uploads/2020/03/15-768x385.png 768w" sizes="(max-width: 1338px) 100vw, 1338px" /> 

To sum things up, we defined the tables, gave permissions and tracked relationships. Hasura already prepared our API for us at this point. You can head to the **GRAPHIQL **tab and explore the API. There is one last thing to do before we move on. We need a way to query the questions that hasn’t been answered by the user. To do that, we can create a PostgreSQL function with an argument. Hasura will again magically track this function and expose it over GraphQL API. Head to **Data** > **SQL. **In here, you can write raw SQL. It can be for creating triggers, views or a function in our case. Write the following code and don’t forget to check **Track this **box. This box will tell Hasura to track this function and expose it over GraphQL API.

    CREATE
    OR REPLACE FUNCTION public.unanswered_questions(userid text)
    RETURNS SETOF questions LANGUAGE sql STABLE AS
    $function$
    SELECT *
    FROM questions
    WHERE id NOT IN (SELECT question_id FROM user_answers WHERE user_id = userid)
    $function$
    

After you click Run, the function will take its place below the tables on the sidebar

<img class="alignnone size-full wp-image-83190" src="https://gbot.dev/wp-content/uploads/2020/03/16.png" alt="" width="416" height="610" srcset="https://gbot.dev/wp-content/uploads/2020/03/16.png 416w, https://gbot.dev/wp-content/uploads/2020/03/16-205x300.png 205w" sizes="(max-width: 416px) 100vw, 416px" /> 

Now that we have deployed Hasura and modeled the data & relationships, we have can move on to the next step. In **Part 2** we are going to prepare Auth and Business Logic by integrating Firebase services. **Part 3** will cover how to to make the Flutter frontend.