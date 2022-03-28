# master.key

## Rails and The Legendary Master.key

When deploying my first application onto Heroku, I discovered there were  quite some challenges as to configuring my repo and Heroku application  properly. The main underlying problem came down to the most recent Rails 5.2 Credentials API.

As of Rails 5.2, `config/secrets.yml`, `config/secrets.yml.enc` and `SECRET_BASE_KEY` are no longer being used to store encrypted keys. From now on, you are to use these files instead: `config/credentials.yml.enc` and `config/master.key`.

## How Do I Use These Credentials?

When using an authentication system in Rails, you might be using `Rails.application.credentials.secret_key_base` to encrypt your JWT token, cookie, or however your system is setup. This works because in your `config/credentials.yml.enc` file you have encrypted keys that secure your authentication system,  database, or API. The thing to remember here is that this file is **OK** to be pushed to your public repo as it is already **encrypted**.

The next important file is `config/master.key` which is where your **RAILS_MASTER_KEY** will be kept. Now this is a very important file that can **never be committed to your source control tool!** I can’t stress this enough because this key will give anyone access to  your encrypted keys and will leave your application vulnerable to  attacks. To avoid this you need to add your `config/master.key` to your *.gitignore* file and this will keep your `master.key` a secret.

### Can I Edit My Credentials?

This straight forward answer is: yes!

Your credentials are encrypted, but with Rails 5.2 there is a way to easily  open your credentials and update them. Run this command:

```
EDITOR="atom --wait" bin/rails credentials:edit
```

This will open your Atom editor (you can use whatever text editor you’re  using) and show your decrypted keys. Notice that I’m using a `--wait` flag which is necessary to edit your credentials without them being  immediately saved. Once you’re happy with the changes made, you can save the file and it will encrypt the file using your `master.key`.

One thing to know is that if you don’t have either of these files, running  the above command will automatically create them for you (thanks Rails).

You can access these secret credentials at any point in your application by using:

```
Rails.application.credentials.key_name
```

Now you’re good to go with your credentials setup!

### Deploy Your Master.key

Deploying an app is always a challenge and if you’re coming into it for the first time it can be twice as hard just trying to get everything configured  properly. Your `master.key` is a key asset when properly hosting your site because it will decrypt your `config/credentials.yml.enc` file. Since your `master.key` is not visible in your repo you need to provide this key as a config variable.

There are two ways of doing that:

- Option 1: Place the `config/master.key` file in the server. You’ll normally want to symlink this file to a  shared folder in the server filesystem. Again, do not version your `config/master.key` file.
- Option 2: create a `RAILS_MASTER_KEY` ENV variable. Rails will detect it and use it as your master key, e.g. in Heroku: `heroku config:set RAILS_MASTER_KEY=<your-master-key-here>`.

Either of these methods will work to get your `master.key` deployed into your apps production configuration. In case you aren’t using a brand new Rails 5.2 app you need to update your `config/environments/production.rb` file:

```
config.require_master_key = true
```

This will tell Rails to ensure that a master key has been made available from either `ENV['RAILS_MASTER_KEY']` or in `config/master.key`.

## Conclusion

Rails credentials are very important and managing them during production of  an application is always a top priority. Deploying an app to Heroku,  Netlify, or any cloud based hosting platform can be tricky so hopefully  this article can solve some of your configuration issues.

Happy Coding!

## References

- https://stackoverflow.com/questions/62011541/using-credentials-yml-with-rails-and-heroku
- https://medium.com/@thorntonbrenden/rails-and-the-legendary-master-key-15c8be7799f1