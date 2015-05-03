---
layout: post
title:  "Step By Step Deploying Rails Application"
date:   2015-04-12
---
<h3>Deploying Rails on Ubuntu Digitalocean, Nginx, Unicorn, Mina</h3>
<p>In the this post I ll try to make I tutorial about deploying of Rails Applications
 by solving issues which I got.
So, I think for rails developers the most painful sections is: <b>Deploying</b> (specially first deploy).
</p>
<p>I ll start from assuming your Rails app works properly in localhost and it have configured on VCS (github etc.)</p>
<p>If you have an account on Digitaloceon, Login and create a droplet like in following way. In the case
  of you have not an account on Digitalocean you can Sign In by using my referral
  <a href="https://www.digitalocean.com/?refcode=02f16ab1f6a4">link.</a></p>
<p>
    <b>Step 1 - Creating new droplet on Digitalocean</b>
</p>
<p>Create your droplet as requirements of your application. For smaller applications it is enough
to use 512 MB RAM - 20 GB SSD plan.</p>
<p><img src="https://dl-web.dropbox.com/get/Herkese%20A%C3%A7%C4%B1k%20Klas%C3%B6r/droplet-size.png?_subject_uid=124692684&w=AACGiCMDOXC_ceSYJ4wPjLMYCk8g5rJ-MKwNHnn4QaPDLQ" />
</p>
The next step is to choose your location. Choose one close to you so that you can have better connection speeds.
<br>
In the next step, we need to choose which OS to use. We ll choose Ubuntu 14.04-64 in this post.
Optionally you can choose Ruby on Rails on 14.04 (Nginx + Unicorn) in 'Aplication' section.
You can add your SSH-key into the droplet as optionally as well.
<p><b> Step 2 - Adding SSH key to digitalocean</b></p>
<p><i>If you already have an SSH key in digitalocean you can choose it and skip this step.</i></p>
<p>
Open new terminal on your local PC and run <b>ssh-keygen</b> command. Make sure your public key has been saved in /home/youruser/.ssh/id_rsa.pub
<br>
Then run <b>cat .ssh/id_rsa.pub</b> and copy display of your command and paste it to digitalocean SSH key content.
</p>
<p>
<b>Step 3 - Adding new  user on Ubuntu Server</b>
</p>
<p>Get your server IP from digitalocean panel and run <b>ssh root@[serverIP]</b> in your local terminal.</p>
<p>So, you have connected to your droplet. Let's start to create new user on your server and install requirements.</p>
{% highlight ruby %}
# adding new user
adduser deployer
visudo

# Search for the line that looks like this:
root ALL=(ALL:ALL) ALL

# add the new user permission below the root permission line
deployer ALL=(ALL:ALL) ALL

su deployer
cd
# you are on new user path rigt now :)
{% endhighlight %}

<p><b>Step 3 - Creating SWAP area on your server</b></p>
<p>This step is not necessary for each system. You can googling about SWAP area. If you decide to SWAP area
 is not necessary for your system. You can skip this step.</p>
{% highlight ruby %}
    sudo fallocate -l 4G /swapfile
    sudo chmod 600 /swapfile
    sudo mkswap /swapfile
    sudo swapon /swapfile
{% endhighlight %}
<p>Run the following command and check your SWAP area if exist and created correctly.</p>
{% highlight ruby %}
sudo swapon -s
{% endhighlight %}
