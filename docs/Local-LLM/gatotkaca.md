# Gatot Kaca Speckit:

```
/speckit.constitution <Command>
/speckit.specify <Command>
/speckit.clarify
/speckit.plan <Command>
/speckit.tasks
/speckit.analyze
/speckit.checklist
/speckit.implement
```

## /speckit.constitution Example:

```markdown
/speckit.constitution Create principles focused on code quality, testing standards, user experience consistency, and performance requirements. Include governance for how these principles should guide technical decisions and implementation choices. The AGENTS.md file is the source of truth. It is a must rule to follow for the project.
```

```markdown
/speckit.constitution Update the constitution. Create principles focused on code quality, testing standards, user experience consistency, and performance requirements. Include governance for how these principles should guide technical decisions and implementation choices. The AGENTS.md file is the source of truth on how the code should be written.
```

## /speckit.specify Example:
```markdown
/speckit.specify Develop Garis Miring, website for helping humans in the world of AI. Users can post their services (example cleaning, repairs), make an admin page to add, remove or update existing services. Make a category for availabe services, for home, apartements or offices. This is dual language website for now, Indonesian as the default and has an English site. The code implementation is in English.


/speckit.specify Develop Garis Miring, forum website for Indonesians. The website is mainly using Indonesian language, the code implementation is in English. Similar to Reddit. It is called subthreads, not subreddit. Users are able to create subthreads. Users are able to comment on those subthreads and have a conversation within that subthread. Users are able to like comments and subthread. 

Users have a personal dashboard on their statistics, with how many threads are made, likes they got, how many comments they made, which post are liked. Users are able to write a forum post called `threads` and are able to share, create bookmark to later read it, like and comment on threads made by other users, also comment on other users comment's. Inside those `threads` are the "conversations" or posts called sub-threads. Similar to a reddit. Users are able to directly message other users. Each users has a "point system", add 1 to the point system if they posts threads or comments. Subtract if they get a "down vote". If a thread is under 6 likes it gets archived and users cannot comment on the thread(read only), but able to continue the conversation by "linking" the old thread. Users are able to vote on their moderators on each threads based on a voting system made by users (with admin insights). If the user is paid by the admin or website owner, the user changes to a staff role. Each threads can be based on universal topics, or sub-threads like location(city based), news, tech, art, fashion, lifestyle, languages, food etc (added later). Users are able to make a new niche threads/topic(s). Shared or similar topics are merged to a single thread (those who are able to merge are admins and staff. Users can report it, but not able to merge). The minimum users can start a new thread is 3 (three) users, they will become the first moderators of that thread. The main language of this website is Indonesian, but the code base is in English (will add English web pages later on).
The main income for this website is based on ads, either from Google or Meta (information is added later), make the ads non intrusive for the users. The users can pay to remove the ads, either monthly or yearly. 


// Change threads to include sub-threads
// Users are not able to spam in the chat room with messages, 1 message for every 200 milliseconds or 5 per seconds. 
// Add flutter support so users are able to chat via an Android or iOS.
// Users are able to make a store to sell products and or services.
```



## /speckit.plan Example:

```markdown
We are already inside an Ash and Phoenix Framework root project folder so we are using the Elixir programming language. Use the default Postgres from Phoenix Framework as the database. The frontend should use liveview with tailwindcss, make use of the Mishka Chelekom Components, create custom Components for the website. Use liveview whenever possible for real-time updates. Use the tidewave MCP for further information about the server logs (but fixing `mix compile` first. Cannot run the server if the project cannot compile). Must refer to the AGENTS.md file for coding standards and code safety. Make sure to run `mix test <test/filename>.ex`, if all test files passed, then use `mix test` to make sure the whole project has minimum bugs. Use `mix usage_rules.search_docs` search docs for bug fixing. Use `mix check` for code security.  
```
