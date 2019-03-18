# Chapter8 Custom user model

Creating our custom user model requires four steps.
1. updating setting.py
2. create a new CustomerUser model
3. UserCreationForm and UserChangeForm
4. Update users/admin.py

Note that we user both null and blank with our age field.
These two terms are easy to confuse but quite distinct.
1. null is database-related.When a field has null = True it can store a database entry as NULL,meaning no value.
2. blank is validation-related ,if blank = True then a form will allow an empty value.where as if blank is blank is False then a value is required.


# Create Super User
Commandline
->python manage.py createsuperuser

# Start up Web server.
Commandline
->python manage.py runserver


# Chapter9 User Authentication
Now that we have a working custom user model we use can add the functionality every website needs: the ability to sign up , log in, and log out users. Django provides to sign up new users.We'll also build a homepage with links to all three features so we don't have to type in the URLs by hand every time.

# Templates
By default the Django template loader looks for templates in a nested structure within each app.So a home.html  template in useers would need to be located at users/templates/users/home.html But a single templates directory within newspaper_project approach is cleaner and scales better so that's what we'll use.
Let's create a new templates directory and within it a registration directory as that's where Django will look for the log in template.



# Chapter10 Bootstrap
Web development requires a lot of skills.Not only do you have to program the website to work correctly,users expect it to look good,too.When you're creating everything from scrach ,it can be overwhelming to also add all the necessary HTML/CSS/ for beautiful site.

Fountunately there's Bootstrap, the most popular framework for building responsive, mobile-first projects.Rather than write all our own CSS and javescript for common website layout features, we can instead rely on Bootstrap to do the heavy lifting.This means with only a small amount of code on our part we can quickly have great looking webistes.And if we want to make common changes as a project progresses,it's easy to override Bootstrap where needed too.
When you want to focus on the functionality of a project and not the design,Bootstrap is a great choice.That's why we'll use it here.

# Tests
in tests.py we check under three things...
1. the page exists and returns a HTTP 200 status
2. the page uses the correct url name in the view
3. the proper template is being used



# Chapter11 Password Change and Reset
In this chapter we will complete the authorization flow of our Newspaper app by adding password change and reset functionality .Users will be able to change their current password or ,if they've forgotten it,to reset it via email.
Initialy we will implement Django's built-in views and URLs for both password change and password reset before then customizing them with our own bootstrap-powered templates and email service.

# Password change
Letting users change their passwords is a common feature on many websites.Django provides a default implementation that already works at this stage.To try it our first click on the 'log in' button to make sure you're logged in .Then negative to the 'password change' page at http://127.0.0.1:8000/users/password_change/
Enter in both your old password and then a new one.Then click the "Change My Password " button.
You'll be redirected to the "Password change succcessful "page located at:
 http://127.0.0.1:8000/users/password_change/done/

# customizing password change
let's customize these two password change pages so that they match the look and feel of our Newspaper site. Because Django already has created the views and URLs for us, we only needed to add new templates.
then you need to add two templatefiles in the registration directory.

# Password Reset
Password reset handles the common case of users of forgetting their passwords. The steps are very similar to configuring password change,as we just did.Django already provides a default implementation that we will use and then customize the templates so it matches the rest of our site.
The only configuration required is telling Django how to send emails.
After all, a user can only reset a password if they have success to the email linked to the account. In production we'll use the email service SendGrid to actually send the emails but for testing purposes we can rely on Django's console backend setting which outputs the email text to our command line console instead.
At the bottom of the setting.py file make the following one-line change.
if you send the password reset email.you'll see like below in your console.
----------------------------------------
Content-Type: text/plain; charset="utf-8"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Subject: Password reset on 127.0.0.1:8000
From: webmaster@localhost
To: aef6ji@gmail.com
Date: Sun, 17 Mar 2019 17:35:57 -0000
Message-ID: <155284415744.9936.8727170330766912538@DESKTOP-60DBTIN>


You're receiving this email because you requested a password reset for your user account at 127.0.0.1:8000.

Please go to the following page and choose a new password:

http://127.0.0.1:8000/users/reset/MQ/54q-4dc06174a4e70e280620/

Your username, in case you've forgotten: naokisan

Thanks for using our site!

The 127.0.0.1:8000 team

-----------------------------------------

# Cuctom Templates
As with "Password change " we only need to create new templates to customize the look and feel of password reset. Create four new template files.
--------------------------------------
touch templates/registration/password_reset_form.html
touch templates/registration/password_reset_done.html
touch templates/registration/password_reset_confirm.html
touch templates/registration/password_reset_complete.html
--------------------------------------


# Chapter12 : Email
At this point you may be feeling a little overwhelmed by all the user authentication configuration
we've done up to this point. That's normal. After all, we haven't even created any  core Newspaper app features yet! Everything has been about setting up custom user accounts and rest.

The upside to Django's approach is that it is incredibly easy to customize any piece of our website. The downside is that Django requires a bit more out-of-the-box code than some competing web frameworks. As you become more and more experienced in web development,the wisdom of Django's approach will ring true.
Currently emails are outputed to our command line console, they are not actually sent to users. Let's change that! First we need to sign up for an account at SendGrid and then update our setting.py files.Django will take care of the rest.Ready?


# Chapter13: Newspaper app
It's time to build our newspaper app. We'll have an articles page where journalists can post articles, set up permissions so only the author of an article can edit or delete it, and finally add the ability for other users to write comments on each article which will introduce the concept of foreign keys.
# Articles to
To start create an articles app and define our database models. There are no hard and fast rules around what to name your apps except that you can't use the name of a built-in app.If you look at the INSTALLED_APPS section of settings.py you can see which app names are off-limits:admin, auth, contenttypes , messages,and staticfiles.A general rule of thumb is to use the plural of an app name- posts,payments,users,etc-.unless doing so is obviously wrong as in the common case of blog where the singular makes more sense.

Next up we define our database model which contains four fields:title,body.date,and author .Note that we're letting Django automatically set the time and date based on our TIME_ZONE setting.For the author field we want to reference our custom user model 'user.CustomUser' which we set in the settings.py file as AUTH_USER_MODEL.

We can do this via get_user_model. And we also implement the best practices of defining get_absolute_url from the biginning and a __str__ method for viewing the model in our admin interface.


# Edit/Delete
How do we add edit and delete options ? We need new urls ,views,and templates.Let's start with the urls. We can take aadvantage of the fact that Django automatically adds a primary key to each database.Therefore our first article with a primary key of will be at articles/1/edit and the delete route will be at articles/1/delete/
