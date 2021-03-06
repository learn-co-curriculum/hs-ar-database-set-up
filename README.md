

## Missed class?

No worries. Get your application set up with the Code Snippets from Learn then follow the steps below to set up your database and add a tweets table.

+ Go to your terminal and run `bundle install` to download your new gems.
+ Then run 
```
rake db:create_migration NAME="create_tweets"
``` 
  to create the file that will hold all the instructions for creating your tweets table.
  * If you get an error message carefully check through all of your files and make sure you put the code snippets in the right place and there are no typos.
+ Migrations are timestamped instructions for your computer that lay out how to build (or modify) a database step by step. 
  * This is important because it allows new developers to join your team at any time, download your code and run the migrations to create a database. 
  * It guarantees that everyone's database is set up exactly the same way no matter when they joined the project.
+ You should now see a `db` (short for database) directory with a `migrate` folder in it holding your timestamped migration.
+ Open up that file and replace the def change method with the following code snippet:

```ruby
  def up
    create_table :tweets do |t|
      t.string :username
      t.string :status
    end
  end

  def down
    drop_table :tweets
  end
```
+ Defining `up` and `down` methods allows us to give explicit instructions for building the table (`up`) and dropping it down (`down`) in case we need to destroy data or make changes.
  * You can see that the `create_table` method that we are calling inside of `up` will create a table "tweets" with columns for username and status, and that these columns will hold data of the type string.
+ To run this `up` method and create the tweets, save your file and type this command into your terminal 
``` bash
rake db:migrate
```

+ You should now see a `schema.rb` file in your `db` directory with a `create_tweets` table.
  * Always check this file after running a migration to see if your table was created properly.
+ Now that that have your database set up, let's add some data!
+ Type `tux` into your terminal to get your Sinatra console started (make sure you are in the root directory of your project file).
+ Once tux has started up type in `Tweet.all`.
+ When you type in `Tweet.all` you'll see a SQL statement like this appear in the terminal:
```sql
SELECT "tweets".* FROM "tweets"
```
+ And this query should return an ActiveRecord object like this `#<ActiveRecord::Relation []>`
  *  You don't have to memorize SQL statemebts, because ActiveRecord creates those queries for us, but check out the SQL Resources on Learn if you want to learn more about SQL .
+ We are returned an empty array, because we haven't created any tweets in the database yet.
+ You can create tweets in the database similar to how you would create an instance of a tweet, but instead of passing in two arguments for username and status, you pass in one argument - a hash with these attributes. Like this:

```ruby 
tweet = Tweet.new({:user => "Vanessa", :status => "Tweet!"})
tweet.save
```

+ The `tweet.save` is really important because that is the command that adds your new tweet instance to your database. If you don't say `tweet.save` your tweet won't be saved! 
  * You can see the SQL statement that adds our tweet to the database when we do tweet.save:
  
  ```sql
  INSERT INTO "tweets" ("status", "user") VALUES (?, ?)  [["status", "Tweet!"], ["user", "Vanessa"]]  
  ```
+ Go ahead and create a couple of more tweets then do `Tweet.all` again.
  *  You should now get and `ActiveRecord::Relation` object with an array holding all of the tweet objects you created. Something like this:

  ```bash
  #<ActiveRecord::Relation [#<Tweet id: 3, user: "Vanessa", status: "Tweet!">, #<Tweet id: 4, user: "Danny", status: "Tweet tweet tweet!">, #<Tweet id: 5, user: "Vanessa", status: "MOAR tweets!!!">]>
  ```
+ Now let's see if your new tweets show up in the browser.
+ Type `exit` to get out of tux and then type `rackup` to start up your server. You should see your tweets at `localhost:9292/tweets`.
+ We need to make one final modification to the application to get our tweets form working.
+ You've set up your `post '/tweets'` route to create instances of Tweets in the standard way, but now we need to feed the tweet attributes to `.new` as one hash argument (instead of two).
+ We can do this exactly the same way that we created tweets in `tux`, but our hash should include the params from the form (not hard coded strings).
+ Your `post '/tweets'` route should now look like this:

```ruby
post '/tweets' do
  tweet = Tweet.new({:user => params[:user], :status => params[:status]})
  tweet.save
  redirect '/tweets'
end
```
+ Stop and restart your server and then refresh `localhost:9292/tweets` and create some tweets! 


<p data-visibility='hidden'>View <a href='https://learn.co/lessons/hs-ar-database-set-up' title='Missed class?'>Missed class?</a> on Learn.co and start learning to code for free.</p>
