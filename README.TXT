This is the source code as of December 15, 2022. Some parts have been removed due to them being irrelevant to most people (e.g. a deployment program). Although it's easy to get the basics up and running, you will probably have to make many changes for it to be completely functional.

This requires Linux or Windows + WSL. It might also work on Mac, but I haven't tried.

Postgresql (13+) and redis are required. If you're on windows, you will need to install WSL, and then install redis with WSL.

1. Create a PG user, DB, and create a file called "config.json" in services/api. Put this in it (replacing the DB, User, and Pass with your credentials):

{
    "knex": {
	"client": "pg",
        "connection": {
        "host": "127.0.0.1",
        "user": "postgres",
        "password": "postgres",
        "database": "db_name_here"
        }
    }
}

2. Install nodejs, go (lang), and dotnet 6. go into the services/api directory in a terminal, run "npm i", then run "npx knex migrate:latest".

3. Go into the services/Roblox/Roblox.Website folder and rename "appsettings.example.json" to "appsettings.json". Put in your DB info and any other configurable things. Also make sure to edit the "Directories" stuff (change "/home/my_username/source-code/" to the exact path of the unzipped source code, i.e. the path this README file is in)

4. Go into the "services/Roblox/Roblox.Website" folder in a terminal, and run "dotnet run". If everything is successful, you should be able to visit the site at "http://localhost:5000/".

5. Start up the "admin" service by opening a new terminal, going into the "services/admin" folder, and running "npm i" then "npm run dev".

6. Open "services/2016-roblox-main/docs/get-started.md" and follow the guide for setting up 2016-roblox (this is the frontend). You should change the "https://{0}.roblox.com{1}" api format to "http://localhost:5000/apisite/{0}{1}"

7. Register an account, then copy your user id and replace the "12" in "OwnerUserId" (inside appsettings) with your user id. ctrl+c the "dotnet run" command to close it, then run it again to start the site back up. You should now be able to go to "http://localhost:5000/admin/" for admin stuff.

8. In order to upload things, you will have to start up the "asset validation service". You can do this by going into "services/AssetValidationServiceV2" in a terminal and running "go run main.go".

Note that the "game-server" program will probably need a lot of edits to actually work as a game service and/or render service.