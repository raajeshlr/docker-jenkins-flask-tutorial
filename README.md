This repo uses Jenkins for CI/CD.

##### This uses local system hardware

YouTube : https://www.youtube.com/watch?v=Kr70uvMKsyk&t=326s

Full article : https://medium.com/sfu-cspmp/machine-learning-deployment-a-storm-in-a-teacup-10541ec3b0d6

Software :

Docker : https://docs.docker.com/desktop/windows/install/

Jenkins : https://updates.jenkins-ci.org/download/war/

ngrok : https://ngrok.com/download

How to install Jenkins : https://www.blazemeter.com/blog/how-to-install-jenkins-with-a-war-file

Go to Jenkins folder : C:\Program Files (x86)\Jenkins

Run below in cmd:

java -jar jenkins.war (let this terminal be open)

#### then go localhost:8080

Install Java if you don’t have : https://www.java.com/download/ie_manual.jsp

Once Jenkins installed :

##### Create Job

1- Create new job, name it docker-jenkins-flask, select freestyle project, click ok.

![Create Jobs](https://github.com/emlopsinfy/docker-jenkins-flask-tutorial/blob/188708a6c58d805a92090489b1ccd4c696225aca/Images/Create%20Jobs.PNG)

2- Select Git as Source code management, add git clone url in repository url.

![Create Jobs](https://github.com/emlopsinfy/docker-jenkins-flask-tutorial/blob/f98a81322f808a5c08b12e2761d6fd36876e497a/Images/Add%20Repo%20URL.PNG)

3-Go down to the build section, add execute shell, add below commands

docker build --pull -t deploy .
docker run -p 5000:5000 deploy

![Create Jobs](https://github.com/emlopsinfy/docker-jenkins-flask-tutorial/blob/f98a81322f808a5c08b12e2761d6fd36876e497a/Images/Add%20Shell%20Commands%20on%20Build.PNG)

save it

Now our job is created.

##### Build it

Just click build now on the left pane.

![Create Jobs](https://github.com/emlopsinfy/docker-jenkins-flask-tutorial/blob/f98a81322f808a5c08b12e2761d6fd36876e497a/Images/Build%20Now%20After%20Job%20Created.PNG)

you must be inside dashboard -> docker-jenkins-flask

If you build is failing, look at output console, and if the error command sh not found, do the below.

This happens because Jenkins is not aware about the shell path.
In Manage Jenkins -> Configure System -> Shell, set the shell path as

> C:\Windows\system32\cmd.exe

https://stackoverflow.com/questions/15135771/hudson-on-windows-error-java-io-ioexception-cannot-run-program-sh

##### It must build your docker image, for me its not working in windows

##### Now creating test.py

import requests

url = 'http://192.168.99.100:5000/api'
feature = [[5.8, 4.0, 1.2, 0.2]]
labels = {
0 : "setosa",
1 : "versicolor",
2 : "virginica"
}

r = requests.post(url, json={'feature':feature})
print(r.json())

When I ran this test.py, I am not getting the result.

This is because shell command step where we mentioned docker build docker run commands are skipped while building.

Installing cygwin

##### Not worked

If it is working, Whenever there is a push to Git, Jenkins creates a docker image.

##### Architecture

![Create Jobs](https://github.com/emlopsinfy/docker-jenkins-flask-tutorial/blob/f98a81322f808a5c08b12e2761d6fd36876e497a/Images/Architecture.PNG)

##### Adding webhooks for CI

![Create Jobs](https://github.com/emlopsinfy/docker-jenkins-flask-tutorial/blob/f98a81322f808a5c08b12e2761d6fd36876e497a/Images/Adding%20Webhooks%20on%20Github.PNG)









