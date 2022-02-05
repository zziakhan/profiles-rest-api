# TECHNOLOGIES

The technologies we're going to use to build our REST API are
Virtual box, vagrant, Python, Django, the django Rest Framework
,VS code, git and the Mod header chrome extension all of these
work togather to produce our working Rest API.

# CATEGORIES
- DEVELOPMENT SERVER
- APPLICATION CODE
- TOOLS

1. DEVELOPMENT SERVER 
```
Running code on our local machine:
- Difficult to work collaboratively
- Different software on windows , Mac, etc.
- Conflicts with other apps we use
- Clogs up our system with dev tools
- Different OS from the server
```
As we write our code will want to test it on our machine.
Many developers run code directly on thier local operating system.
This may be the quickest way to get going but it can create a number of problems when working on real world apps such as making collaborations with other developers extremely difficult problems.
problem to run our our app on different operating systems such as Windows and Mac.
Issues with other development apps conflecting with applications we have installed on our machine
it can clogs up our machine with dev tools and packages.
And finally we're running our code on a completely different operating system then we'll be using on the production server.

That's why professional programmmers always run their code on a local development server to isolate their code from the local desktop.
It's the best practice.

So we'll be doing it here in this course using the following tools the first tool we'll use is vagrant

# vagrant
vagrant allows us to describe what kind of server we need for our app.we can then save the config as a vagrant file which allow us to easily reproduce and share the same server with the other developers once we've told favorite.
What kind of server we need it will use an application called virtual Box to create our virtual server exactly as we describe this means our application code and requirements are installed and running on a virtual server completely isolated from our local machine.

This has many benefits such as 
```
Running code in virtual dev machine:
- Easy to share the server with others
- Exactly same version of all requirements
- Run exactly the same software as a real production server
- Easily create and destroy the server as needed

```
it makes it easier to share code with others regardless of what operating system we're running our code on.
we'll have exactly the same version of all the requirements for our app.

we can test our code using exactly the same operating system and the requirements that will be used on a real production server.

And finally we can easily create and destroy the server as we need making it easier to clean up the 

2. APPLICATION CODE
second set of tools we're use to write our application code to build our app.

3. TOOLS
VS CODE
git(Industry standard version control system)
we're also going to use git git is version controll system and help us track the changes that we make to our code.

## mod Header(Easily modify HTTP Headers)
And finally we're going to use going to use a Chrome browser extension called mod header which allow us to modify the Http headers when we are testing our API 

# Docker vs. Vagrant
I'm going to explain the difference between these two technologies and it might make sense to use one instead of other
## What they have in common
```
- Virtualization technology

```
both vagrant and docker are similar in that they use virtualization technology to isolate the application from machine that is running on.
However they both do this in different ways and each are better suited to different usecases.

## what is Docker?
```
- Open source containerization tool
- Run app in light-weight image
```
docker is an open source containerization tool that allows you to run application in a light weight image.
it works by creating somthing called docker file that contains all the steps required to build the image to run your application during the build stage.
### steps to build the image
```
FROM alpine
COPY ./requirements.txt /requirements.txt
RUN apk add --update --no-cache postgresql-client jpeg-dev
RUN pip install -r  /requirements.txt

RUN mkdir /app
WORKDIR /app
COPY ./app /app
```
during the build stage Docker install all the dependencies and code required to run your app.
An image is typically based on a very lightweight stripped down version of the linux operating system.
once you have your image you can use it run your applicatiion on your local development machine or deploy to a production server beacuse docker is designed to run in production.
## Docker Limitations
it has a steep learning curve compared to vagrant Docker.
Also only has limited versions avaliable for home edition of the window operating system.

## Vagrant
```
Manage virtual development environments
No "out-of-the box" virtualization technology
```
Vagrant is a tool for managing virtual development environments.
However it doesn't come  with any virtaulization technology out of the box 
vagrant work by using called hypervisor(i.e virtualbox) such as Virtual box.

## virtual box:
A tool that's used to run virtual machines on a computer with a vagrant you create a vagrant file which contains all of the instructions for creating your development server vagrant then uses the hypervisor to create and configure the server on your machine.

### BENEFITS OF VAGRANT
```
- Steamlined but complete version of Linux OS
- Easier learning curve vs. Docker
- Wide range of support for operating system
- Runs on any machine that supports Virtualbox

```
A vagrant server usually consists of a slimmed down but complete version of linux operating system such as Ubuntu because vagrant isn't designed to run in development.
it runs on vagrant should run on every machine in which you can install and use virtual box.

## Docker vs. Vagrant
You would use Docker if you're looking to streamline your workflow from devlopment to production and all of your developers on your projects use a supported operating system vagrant is preferred if you're just getting started or if you're looking for development environment that's supported on a wider range of operating systems.

# Creating a development server
In this section we're going to create local development server that we can use to run and test our API as we build it we're going to create a development server using a tool called vagrant. Vagrant allows you to define the type of server you need for project as vagrant file and store this file with the source code of our project you can create a new vagrant file by using the vagrant CLI tool.
lets go ahead do that for our project.
## Creating a vagrant file
```
vagrant init ubuntu/bionic64
```

You use the CLI tool by loading up the terminal or viewing windows to get bash and typing the vagrant init and the type of server that you want to create. so we want use ubuntu/bionic64.what this does is initialize our project with the new  vagrant file and it bases our on the ubuntu bionic64 base image.
These images are publicly available in the vagrant catalog box if you hit enter.
Then what it will do is it will create a new file in our project.
You can see that most of it's being commented out and it's just template for a server that we can use to run our project.
So all this will do if you run this now would be create a basic vanilla Ubuntu image or server based image .
That's how you create a simple vagrant file for your project.

## configuring our vagrant box
Now that we have a template vagrant file for our project we can go and customize this for our particular requirements that we need for our development server.
```
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.
 
  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/bionic64"
  config.vm.box_version = "~> 20200304.0.0"
 
  config.vm.network "forwarded_port", guest: 8000, host: 8000
 
  config.vm.provision "shell", inline: <<-SHELL
    systemctl disable apt-daily.service
    systemctl disable apt-daily.timer
  
    sudo apt-get update
    sudo apt-get install -y python3-venv zip
 
    touch /home/vagrant/.bash_aliases
    if ! grep -q PYTHON_ALIAS_ADDED /home/vagrant/.bash_aliases; then
      echo "# PYTHON_ALIAS_ADDED" >> /home/vagrant/.bash_aliases
      echo "alias python='python3'" >> /home/vagrant/.bash_aliases
    fi
  SHELL
 end
```
Now we have a configured vagrant file we can go ahead and start our vagrant box on our machine.
You use vagrant and by using the terminal or git bash and you can start the vagrant server for our project by changing to our project location.
And typing vagrant up what this will do is it will download the base image that we've specified in our vagrant file and then it will use virtual box to create a new virtual machine and then run our provisioning script when it starts the machine.
we can then use our development server and connect to it using the vagrant tool which we'll show you right after the machine has been created.
Once the vagrant box has been started we can then connect to the vagrant server by using the vagrant ssh command since our box is completely different isolated box on our machine.So as guest operating system.
we need to connect to it using ssh the way you do this is you type vagrant ssh and then vagrant will handle the connection to the machine for you.
You can see that you're on the machine because the input changes for you git bash or your terminal window.
You can see that we're now on vagrant at Ubuntu bionic to disconnect from the machine simply type exit and this will take you outside the machine back onto your local machine.
So if you type vagrant ssh and you connect to the machine any command that you run in the terminal while you're connected to the machine will be run on your guest operating system or the development server insteadof your local machine.
So that's how you connect to the server with vagrant.

Now our development server up and running i'm going to explain about how we can use our developments when we're working on our project.
So because the development server is a virtual machine on our computer by default the far system is not synchronized.
That means that all of the files on our development server are different from the files on our local machine vagrant works by creating a synchronized the directory on our vagrant server that updates itself with all of the files in our local project.
Every time we make changes if you load up the terminal or the git bash window and then connect to our vagrant server I'll explain to you how this work you're connected to the server type
```
vagrant@ubuntu-bionic: cd /vagrant
```
this will switch you to the vagrant directory on our server.
Now every thing in this vagrant directory is synchronized with every thing in our project folder.
so if we create a new file in our vagrant project code test so we do touch test.txt
```
touch test.txt
```
touch is for creating empty files and you can see that test.txt is automatically listed in our project folder local machine.
So all of the files are synchronized.
If we go and remove this text.txt file from our project in editor and the go back to our terminal and type ls you can see that the file has been deleted.
So the synchronization works both ways from the server to our host and from our host to the server.
Okay so how do you run code from our project in our vagrant server.
Make sure we're connected to our vagrant server 
finally lets go ahead and commit and push our changes to github.
So in order to use git we need to switch back to our local machine terminal.
Lets us type
```
git add .
git commit -am "Configured Vagrant and setup hello world script"
```
Now lets push these files to github by doing
```
git push -f origin main
``` 
This will push all  the latest changes that we've made to github so they'll be accessible in our project.

# Create Python virtual Environment
We need to create a python virtual environment so we can install aall the dependencies such as Python, django and django rest framework.
I am going to show you how to create python virtual environment on the vagrant server, so connecting to the server by typing
```
vagrant ssh
```
and change directory to vagrant direcctory.

```py
vagrant@ubuntu-bionic:/vagrant$ python -m venv ~/env
```
what this does is it creates a new file in our vagrant home directory called env and creats pthon environment there.
Now the reason I do it here because I don't want this environment to synchronize with our local machine. So if ever you need to destroy and recreate the the vagrant over from scratch you can do that with a fresh python virtual environment.This is why i specify this '~' here because it will create the environment in the home directory of our vagrant server as opposed to the vagrant folder which is synchronized to our local machine.
so we activate and deactivate the virtual environment.
To activate the virtual environment as you type source and then path to the activate the script with in our environment.
So in our case this path would be in the home directory or ~/vagrant.
```
vagrant@ubuntu-bionic:/vagrant$ source ~/env/bin/activate
```
that's how you activate and deactivate the virtual environment.

Next we're going to test the changes we've made to our django project.
You can easily test changes that you made to a Django project using the Django development server
So django comes with a handy development server that we can test our changes in the browser as we make them to the project.
The way you start the Django development web server is you connect to your vagrant box.

## create our user database model
The first model that we're going to create for our project is the user profile model out of the box.
Django comes with the default user model that's used for the standard authentication system and also the Django admin.
We're going to override this model with our own custom model that allow us to use an email address instead of the standard user name that comes with the Django default model.
These are the standard base classes that you need to use when overriding or customizing the default Django user model.
```model.py
from django.db import models
from django.contrib.auth.models import AbstractBaseUser
from django.contrib.auth.models import PermissionsMixin
from django.contrib.auth.models import BaseUserManager


class UserProfileManager(BaseUserManager):
    """ Manager for user profiles """
    def create_user(self,email,name,password=None):
        """" Create a new user profile """
        if not email:
            raise ValueError("User must have an email address")
        
        email = self.normalize_email(email)
        user = self.model(email=email, name=name)
        user.set_password(password)
        user.save(using=self._db)
        return user
    
    def create_superuser(self,email,name,password):
        """ Create and save a new superuser with given details """
        user = self.create_user(email,name,password)
        user.is_superuser = True
        user.is_staff = True
        user.save(using=self._db)
        return user

class UserProfile(AbstractBaseUser, PermissionsMixin):
    """ Database model for users in the system  """
    email = models.EmailField(max_length=200, unique=True)
    name = models.CharField(max_length=255)
    is_active = models.BooleanField(default=True)
    is_staff = models.BooleanField(default=False)
    
    objects = UserProfileManager()
    
    USERNAME_FIELD = 'email'
    REQUIRED_FIELDS = ['name']
    
    def get_full_name(self):
        """ Retrieve full name of user """
        return self.name
    
    def get_short_name(self):
        """ Retrieve short name of user """
        return self.name
    
    def __str__(self):
        """ Return the string representation of our user """
        return self.email
    




```
we need to define model manager that we're going to use for our the objects.
And this is required for because we need to use our custom user model with Django CLI.
So Django needs to have a custom model manager for the user model so its know how to create users

## Add a user model Manager

Now that we have our custom user model we can go ahead and create a manager.
So Django knows how to work with this custom user model in the Django come online tool.

Next we're going to do something called normailize the email address.Now what normalizing in the email address does is t makes the second half of the email address.

## set our custom user model

Now that we've created our custom user modeland our custom user model manager we can configure Django project to use this is as default user model instead of the one that's provided the Django.
You set the User model in the settings.py file of the project.
you create a new settings line 

```settings.py
AUTH_USER_MODEL = 'profiles_api.UserProfile'
```
Thats how you configure the custom user model in Django.



## What is an APIView

In this section i'm going to introduce you to API Views.The Django Rest framework offers a couple helper classes we can use to create our API endpoints.
### Django REST Framework Views
1. APIView
2. ViewSet

Both the classes are slightly different and offers thier own benefits. In this presentation I'm going to introduce you through the API view the APIView is the most basic type of view we can use to build our API.
```
1. APIView
 - Describe logic to make API endpoint
```
It enable us to describe the logic which makes our API endpoint.

## What are APIViews?
```
- Uses standard HTTP methods for functions
- Give you the most control over the logic
- Perfect for implementing complex logic
- Calling other APIs 
- Working with local files
```
An APIView allows us to define a functions that match the standard HTTP 
```
- GET -> Return one or more items
- POST -> to create an item
- PUT -> Update an item
- Patch -> partially update an item
- DELETE -> Delete an item
```
By allowing us to customize the function for each HTTP method on our API urls. API uses Give us the ost control over our application logic.
This is perfect in cases where you need to do something a little bit different for simply updating objects in the database.
Such as calling other APIs or working with local files so when should you use API vs a lot of the time.

## when to use APIViews?

```
- Need full control over the logic 
- Processing files and rendering a synchronous response 
- You are calling other APIs/services
- Accessing local files or data
```
a lot of the time it will depend on the personal perference as you learn more about the Django rest framework.
Here are some examples of when it might be better to use APIViews whenever you need full control over your application logic such as when you're running a very complicated algorithm or updating multiple data source in one API call or when you 're processing files and rendering a synchronous response maybe you're validating a file and returning the result in the same call another time you might use it when you're calling other external API's or services in the same request.
so that's the basic overview of the APIView.

## Response

So one of these get, post,put,patch,delete must return a response object so Django rest framework is expecting to return a response which it will then output as part when the API is called.
response needs to contain a dictionary or a list which it will the output as part when the API is called.
So it converts the response object to Json and in order to convert it json it needs to be either be a list or a dictionary.


## Serializer
Now that we have created our API view we can go ahead and add a serializer to it. Serializer is a feature from Django REST Framework that allows you to easily convert data inputs into python objects and vice versa is kind of similar to django form which you define and it has the various fields that you want you to except for the input for your API. So if you're going to add post or update functionality to our Hello API View then you need to create serializer to receive the content that we post to the API.
Serializer is function very similar to Django Forms.
All you do is you define the serializer and then specify the fields that you want to accept in your serializer input.
so we are going to create a field code name and this value that can be passed into the request that will be validated by the serializer. so serializer also take care of validation rules.

```views.py
# APIView class
from django.shortcuts import render
from rest_framework.views import APIView
from rest_framework.response import Response

from rest_framework import status
from profiles_api import serializers

class HelloApiView(APIView):
    """" Test API View """
    serializer_class = serializers.HelloSerializer
    def get(self, request, format=None):
        """ Returns a list of APIView features """
        an_apiview = [
            'Uses HTTP methods as function (get, post,put,patch,delete)',
            'Is similar to a traditional Django View',
            'Gives you most controll over you application logic',
            'Is mapped manually to URLs'
        ]
        return Response({'messages': 'Helllo!','an_apiview': an_apiview})
    
    def post(self, request):
        """ Create a hello message with our name """
        serializer = self.serializer_class(data=request.data)
        
        if serializer.is_valid():
            name = serializer.validated_data.get('name')
            message = f'Hello {name}'
            return Response({'messages': message})
        
        else:
            return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
        
    def put(self, request,pk=None):
        """Handle updating an object"""
        return Response({'messages': 'PUT'})
    
    def patch(self, request,pk=None):
        """ Handle a partial update of an object """
        return Response({'messages': 'PATCH'})
    
    def delete(self, request,pk=None):
        """ Delete an object """
        return Response({'messages': 'DELETE'})

```

```serializers.py
from rest_framework import serializers


class HelloSerializer(serializers.Serializer):
    """ Serializes a name field for testing our APIView """
    name = serializers.CharField(max_length=10)
```
```urls.py
from django.urls import path
from profiles_api import views
urlpatterns = [
    path('hello-view/', views.HelloApiView.as_view())
]
```

# Introduction to Viewsets

```
Django REST Framework Views
1. APIView
2.ViewSet
```
At the beginning of the last section i mentioned that django rest framework of is two classes that helps us write the logic for our api.
The APIView and ViewSet classes.

## What are ViewSet ?
```
- Uses model operations for the functions
- Take care of a lot of typical logic for you
- Perfect for standard database operations
- Fastest way to make a database interface 
```

```
list -> Getting a list of objects
create -> Creating new objects
retrieve -> Getting a specific object
Update -> Updating an object
partial_update -> Updating part of an object
destroy -> Deleting an object
```
Just like APIView Viewsets allow us to write the logic for our endpoints. However, instead of defining functions which mapped to HTTP methods Viewsets excepts function that maps to common api object actions.
## When to use ViewSets?

```
- A simple CRUD interface to your database
- A quick and simple API
- Little to no customization on the logic
- Working with standard data structures
```
So I explained previously the functions that you add to the ViewSet is are a little bit different from APIView In APIView you add functions for the particular HTTP methods that you want to support on your endpoint for example HTTP POST, HTTP PUT, HTTP PATCH and HTTP DELETE for a ViewSet you add functions that represents actions  that you would perform on a typical api so the action that we're going to add is the list so a list is ypically HTTP get to the root of the endpoint to our ViewSet.

```views.py
                <!--views.py   -->
from rest_framework import viewsets

class HelloViewSet(viewsets.ViewSet):
    """ Test API ViewSet """
    serializer_class = serializers.HelloSerializer
    
    def list(self, request):
        """ Return a hello message """
        a_viewset = [
            'Uses action (list,create,retrieve,update,partially_update,destroy)',
            'Automatically maps to URLs using Routers',
            'Provides more functionality with less code'
        ]
        return Response({'messages': 'Hello!', 'a_viewset': a_viewset})
    
    def create(self, request):
        """ Create a new hello message """
        serializer = self.serializer_class(data=request.data)
        
        if serializer.is_valid():
            name = serializer.validated_data.get('name')
            message = f'Hello {name}!'
            return Response({'messages': message})
        else:
            return Response(
                serializer.errors,
                status = status.HTTP_400_BAD_REQUEST
            )
            
    def retrieve(self, request,pk=None):
        """ Handle getting an object by its ID """
        return Response({'http_method': 'GET'})
    
    def update(self, request,pk=None):
        """ Handle updating an object"""
        return Response({'http_method': 'PUT'})
    
    def partially_update(self, request,pk=None):
        """ Handle updating part of an object """
        return Response({'http_method': 'PATCH'})
    
    def destroy(self, request,pk=None):
        """ Handle removing an object """
        return Response({'http_method': 'DELETE'})
```

```urls.py
                <!--urls.py   -->
from django.urls import path, include

from profiles_api import views

from rest_framework.routers import DefaultRouter

router = DefaultRouter()
router.register('hello-viewset', views.HelloViewSet, basename='hello-viewset')

urlpatterns = [
    path('hello-view/', views.HelloApiView.as_view()),
    path('', include(router.urls))
]
```

## PLAN OUR PROFILE API

Now that we have to know how to make simple API we can move on to building our profiles api.
I'm going to explain the specification of what we're going to build. Let's starts by listing some basic requirements our profile api is going to be handle the following 
## BASIC REQUIREMENTS 
```
1. Create new profile
    - Handle registration of new user
    - Validate profile data

2. Listing existing profiles
    - Search for profiles
    - Email and name

3. View pecific profiles
    - Profile ID

4. Update profile of logged in User
    - change name, email and password

5. Delete profile
``` 
# API URLs

```
/api/profile/ 
-> list all profiles when HTTP GET method is called
-> create new profile when HTTP POST method is called

/api/profile/<profile_id>/

-> view specific profile detials by using HTTP GET method
-> update object using HTTP PUT / PATCH
-> remove it completely using HTTP DELETE
```
## create user profile serializer
Let's start by creating our user profile api by creating a serializer for our user profile object.

```
from rest_framework import serializers

from profiles_api import models


class HelloSerializer(serializers.Serializer):
    """ Serializes a name field for testing our APIView """
    name = serializers.CharField(max_length=10)
    

class UserProfileSerializer(serializers.ModelSerializer):
    """ Serializes a user profile object """
    class Meta:
        model = models.UserProfile
        fields = ('id', 'email', 'name', 'password')
        extra_kwargs = {
            'password': {
                'write_only': True,
            },
            'style': {'input_type': 'password'}
        }
        
    def create(self, validated_data):
        user = models.UserProfile.objects.create_user(
            email = validated_data['email']
            name = validated_data['name']
            password = validated_data['password']
        )
        
        return user
```
## Create Profiles ViewSet
```
class UserProfileViewSet(viewsets.ModelViewSet):
    """ Handle creating and updating profiles """
    serializer_class = serializers.UserProfileSerializer
    queryset = models.UserProfile.objects.all()
```
## Register profile Viewset with URL router

```
from django.urls import path, include

from profiles_api import views

from rest_framework.routers import DefaultRouter

router = DefaultRouter()
router.register('hello-viewset', views.HelloViewSet, basename='hello-viewset')
router.register('profile', views.UserProfileViewSet)

urlpatterns = [
    path('hello-view/', views.HelloApiView.as_view()),
    path('', include(router.urls))
]
```
Unlike the 'hello-viewset' that we register previously we don't need to specify the a basename argument then this is because we have in our view set a queryset object if you provide the queryset then Django REST Framework can figure out the name from the model that's assign to it. so you only need to specify the basensme if you're creating a view set that doesn't have a queryset or if you want to override the name of the queryset that associated to it.

## create permission classes
 Okay so now we have fully functioning profile REST api. 
 The way you define permission classes is you add a has object permissions function to the class which gets called every time a request is made to the api that we assign permission to the class.
 This function will return True or False .To determine whether the authenticated user has the permissionto do the change that they're trying to do.
 when you authenticate a request in Django REST framework it will asign the authenticated user profile to the request and we can use this to compare it o the object that is being updated and make sure they .
 so when the user makes a request we're oing to check if the request is in the safe methods.If it is in the safe methods we are just going to allow the request to go through. Otherwise if it's not in the safe method. So they're using an update or delete or something like that then will return he result of obj.id equals to request..user.id This way it will eturn True if the user is trying to update thier own profile or otherwise it will return false.
 Okay so thats how we create a custom permisssions with the Django rest framework.
```
from rest_framework import permissions


class UpdateOwnProfile(permissions.BasePermission):
    """ Allow user to edit thier own profile """
    
    def has_object_permission(self, request,view, obj):
        """ check user is trying to edit thier own profile"""
        if request.method in permissions.SAFE_METHODS:
            return True
        return obj.id == request.user.id
```

## Add authentication and permissions to the viewset
Now that we have our update own profile permissions class we can go ahead and configure our viewset to use this permission.
At the top of file we're going to add some imports and the first import we're going to import the token authenticationfrom the rest framework so type from 
The Token Authentication is type of authentication we use for users to authenticate themselves with our API it works by generating a random token string when the users login and then every request we make to that we need to authenticate we add this token string to the request and hat's effectively a password to check that every request made s authenticated correctly we're going to configure this on our viewset and then 
So every request that gets made it gets passsed through our permissions to profile and it checks.This has object permissions function to see wether the user has permissions to perform the action they trying to perform.

## Add search profiles features
Next we're going to add the ability to search profiles by a specific name or email address by adding the 

## create login api viewset

## Set token header using ModHeader Extension
Next let's test our authentication token using the mod headers chrome extension.
Now the way that the token authentication works is every single request that's made to our api has a HTTP Header.
What we do is we add the token to the authorization header for the requests that we wish to authenticate.













