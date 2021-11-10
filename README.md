#Blitz RealWorld

[React](https://reactjs.org) + [NextJS](https://nextjs.org) + [Prisma](https://www.prisma.io)

A FullStack [RealWorld Starter App](https://codebase.show/projects/realworld?category=fullstack)

##Set Up Your Computer

You need Node.js 12 or newer. You can verify this by running `node -v` in your terminal. If you don't have Node or need a newer version, we recommend using a node version manager like [fnm](https://github.com/Schniz/fnm) so you can change node versions for each project.

##Install Blitz

Run `yarn global add blitz`

##Create a New App

Choose Typescript, Full and Yarn...

`blitz new myAppName`
`cd myAppName`
`blitz dev`  

View your brand new app at [http://localhost:3000](http://localhost:3000)

If delete your app folder, use a new terminal window when running `blitz new myAppName` to avoid conflicts in your virutal .env


##Add RealWorld model to your app

To create a RealWorld 2.0 site, copy schema.prisma into the db folder. This will provide:

RealWorld tables: User, Article, Content, Tag
Blitz tables: Session, Token, Project, Question, Choice
Neighborhood tables: Org, Event, Service, Item

To run only the simple RealWorld 1.0 schema, copy realworld.prisma to db/schema.prisma.
<!-- changed provider = "mysql" to "sqlite" -->

Generate the model by running:
`blitz generate all project name:string`
Select Yes to run Prisma migrate dev to update your database
Enter a name for the new migration, then restart the server

`Ctrl + c`
`blitz dev`

Then go to [http://localhost:3000/projects](http://localhost:3000/projects)

##Fixed Author Error (line 48)

realworld.prisma originated from [NestJS+Prisma sample](https://github.com/lujakob/nestjs-realworld-example-app/blob/prisma/prisma/schema.prisma), with mysql changed to sqlite.  
With realworld.prisma copied to schema.prisma, an error occurred when running `blitz generate all project name:string` 

Fixed by adding: comments      Comment[] @relation("UserComments")
Inserted: author        User        @relation("UserArticles", fields: [authorId], references: [id])

This solved error in line 48:
// author      User        @relation(fields: [authorId], references: [id])

<pre><code>
Error: Schema parsing
error: Error validating field `author` in model `Comment`: The relation field `author` on Model `Comment` is missing an opposite relation field on the model `User`. Either run `prisma format` or add it manually.
  -->  schema.prisma:48
   | 
47 |   articleId Int
48 |   author    User     @relation(fields: [authorId], references: [id])
49 |   authorId  Int
   | 
</code></pre>
