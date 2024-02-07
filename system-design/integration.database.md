# integration database

Martin Fowler

An integration database is a database which acts as the data store for multiple applications, and thus integrates data across these applications (in contrast to an ApplicationDatabase).

An integration database needs a schema that takes all its client applications into account. The resulting schema is either more general, more complex or both - because it has to unify what should be separate BoundedContexts. The database usually is controlled by a separate organization to those that develop applications and database changes are more complex because they have to be negotiated between the database group and the various applications.

The benefit of this is that sharing data between applications does not require an extra layer of integration services on the applications. Any changes to data made in a single application are made available to all applications at the time of database commit - thus keeping the applications' data use better synchronized.

On the whole integration databases lead to serious problems becaue the database becomes a point of coupling between the applications that access it. This is usually a deep coupling that significantly increases the risk involved in changing those applications and making it harder to evolve them. As a result most software architects that I respect take the view that integration databases should be avoided.

## References

- https://martinfowler.com/bliki/IntegrationDatabase.html


# Integration database and why you should avoid it

Nowadays, it's common for companies to shift towards a microservices  architecture. This approach promises a lot of benefits, but you should  also be aware of common mistakes to avoid while migrating, otherwise you may end up with something even worse than your current system. This  time I'll cover one of such pitfalls - integration database.

## The need for split

Microservices architecture doesn't come for free and makes lots of  aspects of application's lifecycle more complicated, for example:

- bootstraping
- continuous integration
- deployment
- monitoring, alerting, tracing
- needs more planning
- needs more mature engineers

You need to standardize and automate most of the bullet points from  the list above first before you can even consider moving towards  microservices. If it requires so much work to be done in advance, why do companies choose this approach?

## Why microservices?

For me, some of the main reasons for adopting microservices are the following ones:

- organization scaling
- reduction of cognitive load
- promotion of ownership and expertise
- faster time to market

From my experience, there is a limit of people who can work  simultaneously on the same project without stepping on each other's feet all the time. After that limit is reached, adding new team members will only make the situation worse and slow down the team even more. So,  when it happens, companies seek for organization structures and team  topologies that can help them get back their previous velocity.

## Characteristics

Companies want to improve their velocity, so let's analyze why and  how microservices can help with that. Properly designed microservices are:

- well-scoped
- autonomous
- manageable by one team
- easy to deploy
- promote ownership
- embrace new technologies
- increase resilience
- help with scaling

If we think about autonomy and that a microservice should be managed  by one team only, we can see that it may help us with velocity. We may  have a smaller team that will own a microservice related to a specific  domain area and if microservices are autonomous, it means the team won't have (generally) dependencies on other teams to move features from an  idea to production. The smaller size of a team will also  significantly reduce stepping on each other's feet problem.

As each microservice will be owned by only one team, it will promote  ownership. At the same time, ownership will lead to better expertise and quality due to a few simple reasons:

- team members will spend most of their time in that microservice (domain area), so their expertise will improve
- as they will own the microservice, they will know it will be only  them who will develop and support it. Hence they'll try to do it in the  best way possible to avoid ending up maintaining a big ball of mud

## Guidelines

To achieve the mentioned above characteristics and benefits, we need  to follow a set of simple rules while developing new microservices:

- hide internal implementation details and expose clear APIs
- be focused on doing one thing well
- communication between microservices must be done via network calls
- microservices must be able to change independently of each other

Sometimes we lose focus, make mistakes, so from time to time we need  to ask ourselves proper questions to get back on track. To do so I  follow **Sam Newman**'s question:

> Can you make a change to a service and deploy it by itself without changing anything else?

## Common pitfall: Integration DB

One of the most common pitfalls observed by me personally is the integration database. As per **Martin Fowler**, here is the definition:

> An integration database is a database which acts as the data store  for multiple applications, and thus integrates data across these  applications

Let's try to analyze why people sometimes use it and why it's bad and considered as an anti-pattern.

### Pros

It wouldn't be fair to not mention benefits that you may get by using an integration database, though there are not that many: 

- fast integration between services

It may be tempting to use it as changes in one service are reflected  in all others without extra work. That's all about pros. Please leave a  comment if I forgot something and you feel there is more to it.

### Cons

- exposes internal implementation details to clients
- there is no clear API
- changes to schemas/tables by one service can affect other services
- requires a large amount of regression testing
- prevents loose coupling
- no ownership of data
- introduces the fear of change

As you can see from the list above, integration DB goes against the main microservice guidelines. For example:

> hide internal implementation details and expose clear APIs

at the same time, with integration DB we have:

> exposes internal implementation details to clients

The database is a low-level implementation detail of the service that owns it. Clients of that service should not know anything about it. For example, as an owner of a service, you should be able to not only change any schemas/tables in your database but also change the database  itself if needed. You should freely either upgrade it to a newer version or switch to another provider. It can be achieved only if nobody  expects the microservice that owns that database has access to it, including binary logs.

Microservices are usually referred to as **shared-nothing** and it's like that for a reason.

Another violation is:

> there is no clear API

as it goes against the following principle:

> communication between microservices must be done via network calls

Clear API gives you control over an aggregate's state machine as per  DDD. At the same time, the integration database usually shares the same  DB user across different microservices. In that case, there is no  precise or clear API at all. More on it later.

## Example: modifying a column

Let's assume that we have a few teams and each of them owns one or a  few microservices, and all of them are integrated via an integration  database as illustrated below:

![integration database example](https://victor.4devs.io/uploads/images/integration-db/integration-db-cross-team-example.jpg)

There may be plenty of tables in such a database. Let's assume that one of them is the **user** table.

**Question:** as an engineer working on service B in  team 1, can you safely change the structure of a user table, for  example, rename or drop a column?

**Long story short**: No, you can not

Let's try to understand why the answer to such a trivial task as renaming of a column is **no**. 

As per the diagram above, there are six services that all use the  same database. Do we know for sure if any of them use the user table? If yes, do we know how exactly all of them use that table?

If you feel that it's strange to ask such questions to do something  as simple as renaming of a column, you're right, but this is a **"normal"** life if you use an integration database.

So now, the complexity of this simple task has grown from the  necessity to know only about service B and how it uses the user table to the necessity to know how all six services use that table.

To do so, you'll spend extra time checking service A. This will take  you some time, but probably not much as it's owned by your team and you  have access and knowledge about it.

What about four services managed by two other teams? Now you need to  talk to some experts from those teams and ask them about all their  projects as per the diagram to see if they use that table. Do you feel  it? Just to rename a column, you already need to approach two other  teams. It consumes even more time.

After you talked to experts in those two teams, you got to know that  they don't use the user table. You're happy as finally, you can proceed. You open a PR with database migration, CI server runs tests for your  service and everything works perfectly so you deploy it to production.

The next day you come to the office, there are already people from  some other teams waiting for you as you broke their stuff. You may ask  "how?". Let me explain it to you.

It's seldom the case that one person in a big company knows  everything about all teams and projects. So your perception of the  organization with 3 teams was wrong, and in reality, there are more  teams/departments that you happened to not work closely with as per the  diagram below:

![hidden dependencies](https://victor.4devs.io/uploads/images/integration-db/hidden-dependencies.jpg)

So, in addition to teams 1, 2, and 3, there are two more teams (data  engineering and business intelligence) that are now blaming you for  breaking their pipelines.

For example, the data engineering team used a user table in one of  their services to gather data for a recommendation engine. BI team also  used the same table to build reports and provide them daily to C-level  management.

Both of those teams read data directly from the integration database  and had a mapping to that column that you just renamed, so things got  broken.

Do you see how quickly a trivial change can get complicated and get  out of control if you use an integration database? Do you need that  headache?

Based on this simple example, I wanted to show that integration database violates the following principles of microservices:

> hide internal implementation details and expose clear APIs

As we've just seen, integration DB is a shared resource and not  treated as an internal implementation detail in the example above, nor  it provides a clear API. So it's a clear violation of the principle  above. More to it, let's look at the following principle:

> communication between microservices must be done via network calls

Does any other service get information about users via a clearly  defined API provided by service B? No, so it's another violation. In  this case, we don't even have communication between microservices at  all, they only communicate with a database.

One more important point:

> microservices must be able to change independently of each other

I hope after the provided example, it's clear that with the  integration database we violated this principle as well. If you could  change service B independently, you would not need to ask engineers from other teams, nor you had to explain two other teams why you broke their stuff.

If we remember a few principles that are the most important for me in terms of benefits microservices provide, as per the list below:

> - reduction of cognitive load
> - promotion of ownership and expertise
> - faster time to market

we can see that we violated all of them as well. If service B owned a user table, the required knowledge to rename a column would be limited  to service B. In our case, the scope grew so much, that it spun across  multiple teams and multiple services.

As the database is a shared resource, there is no ownership of data,  each team does whatever they think is appropriate, so we can observe  lack of ownership and as a result - lack of expertise.

Faster time to market - I think, it doesn't need any explanation.

This feature could be delivered way faster and without breaking other services if only service B owned the user table and avoided sharing it  with others.

## Example: adding a new table

Usually, when companies have an integration database, there is only  one service that executes database migrations (most commonly it's a  legacy monolith due to historical reasons). Let's assume that in our  case, that service is **A** as per the following diagram:

![run migrations](https://victor.4devs.io/uploads/images/integration-db/run-migrations.jpg)

**Question:** as an engineer working in team 2 on service C, can you independently add a new table?

**Long story short:** no, you can not

Again, you got a trivial task: add a new **location_tree** table and then show its information in service C. To complete it, you need to:

- write a DB migration
- update the mapping to consider the newly added table and return information via a new API endpoint

Should be simple, right?

As we've just figured out, there is only one service capable of running DB migrations and it's service **A** managed by another team. Ok, no problem, we can start their project in our  local environment, add a new DB migration and open a pull request to  their repository.

Do you see what's happening? To add a new table for your service, you need to set up another project in your local environment. Time wasted.

After all, changes are done and you opened a PR, you approach owners  of service A and ask them to review your changes. Unfortunately, they  tell you that they have a big problem in their master branch and till  it's fixed, they won't merge anything at all.

What's next? You have to wait. It also means that no other teams can proceed with their DB migrations, **everyone is blocked** until the issue in service A is fixed.

I experienced a similar issue and it took ~2 days to fix master in a target repository to unblock all other teams. 

So again, integration DB can lead to violation of the following principles and benefits of microservices:

> - faster time to market
> - microservices must be able to change independently of each other

## Example: modifying data

When we think about the integration database and problems it brings  to the table, we should not limit ourselves with classic databases like  MySQL, MongoDB, and so on. Instead, we should keep in mind that many of  those constraints are applied to other types of storage, like Redis,  Memcache, and so on. Let's check one more example (the last one, I  promise). For example, you may have the following setup in your company:

![shared cache - initial state](https://victor.4devs.io/uploads/images/integration-db/shared-cache-1.jpg)

The monolith makes a call to Redis to get some pre-stored  configuration. If the configuration is empty, it makes a call to MySQL  to get more data, recalculates and saves new fresh config to Redis. As  you can see from the flow diagram, config stored in Redis under one key  and inside it has related information A, B, and C.

After some time, the codebase and the company grew and engineers  decided to move towards microservices. To let teams work independently  faster, they decided to split code by copying the monolith first, so  they ended up with the following setup:

![split by copy](https://victor.4devs.io/uploads/images/integration-db/cache-copy.jpg)

What we see here is that now we have a few copies of the monolith,  managed and deployed by separate teams. With time projects evolved and  got cleaned up, so after some months the diagram looks like this:

![evolution that led to a tricky bug](https://victor.4devs.io/uploads/images/integration-db/storage-evolution-bug.jpg)

**Question**: does it still look ok?

**Long story short**: no

It's not easy to notice even from this diagram, not saying about  isolated code bases of each project, but this setup introduced a tricky  to catch & understand bug.

As per the initial flow diagram, monolith (or any of its copies) asks Redis for config that should have A, B, C values inside. If it's a  cache miss, then it recalculates the cache and writes it back.

Do you see the problem already? Ok, lemme clarify it.

If let's say service **D** asks for config from Redis  and there is nothing, it will recalculate it and store it back. But  because service D evolved and some cleanup was done, now it operates  with only properties A and C from config and doesn't need B. So it will  write back A and C, not A, B, and C.

When any other service that needs all 3 properties of the config (or  at least property B) calls Redis to get config, it will get a record  from it as it has properties A and C and it's a cache hit. So that  service (any service except service B) in our example will treat invalid config as valid and won't try to regenerate it, but will just use it  directly instead.

At the same time, one important property B from that config is missing, which leads to a bug. 

You may think that this is a very naive example that won't happen in  real life, but it happened in one company I worked for and took me quite some time to understand the whole global picture to be able to fix it.

Again, because we shared storage, we ended up in a trap, so it's one  more good example of why integration DB (or just schema-less storage) is bad.

A proper solution, in this case, would be to give each service its dedicated Redis.

## Recommendation

I hope by now it's clear that as engineers, we should avoid  integration DB at (nearly) any cost. Instead, our microservices should  communicate with each other by using clear APIs:

- async (events)
- sync (HTTP endpoints)

Keep in mind that it doesn’t matter if services are managed by  different teams or not. The reasoning for having microservices stays the same, so do principles and thus the need to have clear APIs between  them. All that means that even if a set of services is maintained by the same team, you should still avoid integration via a database.

Try to use already mentioned Sam Newman's phrase to check if you're doing things right:

> Can you make a change to a service and deploy it by itself without changing anything else?

If you share the database between **any** services, even managed by the same team, you’re doing something wrong.

## Still not convinced?

If I haven't convinced you yet, let's check what respectable people from the field think about the topic.

For example, from **Martin Fowler**'s blog post about the [integration database](https://martinfowler.com/bliki/IntegrationDatabase.html):

> Most software architects that I respect take the view that **integration databases should be avoided**

Another good example is **Jeff Bezos**' API mandate that has the following points as per **Steve Yegge**'s [post](https://gist.github.com/chitchcock/1281611):

> There will be no other form of interprocess communication allowed: no direct linking, **no direct reads of another team's data store**, no shared-memory model, no back-doors whatsoever. The only  communication allowed is via service interface calls over the network.

## How to prevent it from happening?

The best treatment is prevention. What I mean by that is you need to  be proactive, try to make your peers aware of all problems related to  integration databases to ensure you're all on the same level and the  company as a unit is aligned. Initiate discussions around the topic to  have coding guidelines accepted on a company level. Make it explicit  that the integration database is bad and should be avoided. Share the  knowledge.

## How to fix it if it's already there?

If you already have an integration database in your company, you  still need to explain to people why it's bad first. Please follow the  advice from the previous paragraph. After it's done and there is  understanding of the problem among engineers, come up with a long term  vision for integration database, for example:

- Investigate
- Limit
- Eliminate

Integration DB is not easy and fast to fix, so the solution should be split in iterations as described above.

### Investigate

First of all, you'll need to collect more information and understand  the current state of the integration databases. Give each team a  dedicated DB user for each of their services and ask them to switch.

Those new DB users initially will have the same permissions as a  global/shared user that is probably used now across all teams (this will be improved during the next iterations). After all teams switch, try to limit/delete the global user.

For example, if you have the following setup:

Team 1: service A, B, C

Team 2: service D, E

Team 3: service F

...

Then create users like service_a, service_b, and so on and distribute them to the related teams. By doing so you'll get more visibility and  clarity. Instead of seeing all requests coming from the same global  user, now you'll see requests coming from service-based users. It will  help you to understand what tables are used by what services. It can be  as simple as parsing logs and building some kind of a matrix.

Yet, there may be caveats with this approach. Depending on the domain you work in, some features may be used rarely, for example quarterly.  If you gather logs for one week, you won't see service users using  tables that support those rarely used features.

In this case, it's better to cross-check with teams manually as well. An easy way to do so is to create a file with a list of all tables from the DB and ask teams to mark tables and for what services they're used. It may look like:

![table usage investigation](https://victor.4devs.io/uploads/images/integration-db/table-usage-investigation.png)

We can easily spot all problems based on such a file. For example, we can see that table_1 is used by services A, C, D, and F. This is where  we may have problems and what we need to fix. 

### Limit

After the information is collected, the next step would be to reduce  the permissions of service users. Instead of them having access to all  tables, we need to reduce permissions to work with only listed in the  file or collected via logs tables. For our file we will get the  following set of permissions:

- service_a: table_1
- service_b: table_2
- service_c: table_1, table_3

and so on. By doing that, we will prevent the integration database  from growing even more. If let's say team 3 decides to take a shortcut  and use table_2 directly, they won't be able to do so as their service_f user won't have permissions to read data from that table. It will force them to open a discussion about ownership of the data and development  of new APIs if needed.

### Eliminate

This is the biggest and the most complicated part that doesn't have a clear answer as it's case specific. The main goal here would be to  prioritize problem zones based on solution, effort, and benefit. Those  cases can be solved one by one and will require alignment of teams that  use each of those shared tables.

**Note:** all these steps should be communicated to  engineers, DevOps, and so on. Everyone should be aligned and clearly  understand the direction and long term goal.

## Conclusion

As always, long story short: avoid integration databases and be  proactive in doing so. The best treatment is prevention as it will cost  you way less.

## References

- https://victor.4devs.io/en/architecture/integration-database.html
