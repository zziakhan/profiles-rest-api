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






















