---
layout: post
title: Docker be hard
subtitle: Finishing up PyNance and attempting to containerize it
---

After working a bit on PyNance, I was able to fully complete the project. After cleaning up the
program and adding some documentation/removing redundant code, I've managed to get a working 
version of PyNance pushed to GitHub. The code is still relatively janky with a somewhat
unpopulated spreadsheet (and more importantly no spreadsheet template), but it works as an MVP 
for further feature addition.

I've still got no clue how to approach the automation process. After looking at creating a 
script to generate a Cron Job or System Service, I decided to have a look at Docker. This is 
something I had been putting off for a long time. I had countless issues getting Docker Desktop 
working on my workstation, with errors showing up in relation to WSL2. As it turns out, this 
is a well documented issue (a similar issue was found [here](https://github.com/docker/for-win/issues/8204)).
After disabling WSL2 and uninstalling docker, I managed to force Docker to use the Hyper-V legacy 
backend instead of the WSL2 backend. I'm not entirely sure what the difference is (the quick 
blurb mentions performance...?), but as of now my Docker install finally works. Something I'll 
have to read more about. 

Next up was trying to learn how to build a Dockerfile and a docker-compose.yml file. I understood 
the basic logic surrounding containers in Docker, but the syntax and exact process that Docker 
followed to build and run a container was somewhat foreign to me. After doing a bit of reading, I decided 
to create a basic container from the Python:3.9 image and copied all the program contents into 
the container cache. After installing all dependencies, the container would run main.py, and then 
exit. I have to admit, I was somewhat surprised it worked first try, but I was also puzzled at some behaviour the container was exhibiting (I certainly wasn't expecting it to abruptly exit 
after execution).

A few questions were raised at this point, what is the container doing upon exit? Is it tearing 
itself down? How do I make a Cron job (or something of similar function) out of this? And most 
importantly: Why is my container size 1.03Gb when I only have a few text files and barely any Python
packages. Last I checked, Python is pretty lightweight. Is this a by-product of Docker creating a lot of 
overhead for a performance gain at runtime? This is something I'm going to have to investigate further.