In this lab, we will be playing aroud with a basic gaming application and utilizing a variety of AWS services. This lab is ideal for entry and intermediate level developers.

                              
What will we need?


[Github](https://github.com/signup)- this is where we will contain our code: html, css, javascript, and various image files 
[S3](https://us-east-1.console.aws.amazon.com/s3/home?region=us-east-1)- This is where we will deploy our code to and an empty bucket is needed prior to starting this project
[Codebuild ](https://us-east-1.console.aws.amazon.com/codesuite/codebuild/start?region=us-east-1#)and/or Jenkins is optional if you want to import custom code 
[Code Pipeline](https://us-east-1.console.aws.amazon.com/codesuite/codepipeline/pipelines?region=us-east-1&pipelines-meta=eyJmIjp7InRleHQiOiIifSwicyI6eyJwcm9wZXJ0eSI6InVwZGF0ZWQiLCJkaXJlY3Rpb24iOi0xfSwibiI6MzAsImkiOjB9)- The service we will use to automate and continously deliever the needed code.

So What's Our Budget?

Github is entirely free if using the basic tier, a [pricing calculator](https://github.com/pricing) is also available. 

Most of this will use AWS free tier and should only invoke little to no cost. Feel free to use [AWS pricing calculator ](https://calculator.aws/#/)for more in-depth information.

S3 bucket cost 
![S3 bucket cost](https://github.com/user-attachments/assets/f55519f4-024b-4344-9bcc-a4f150adadbf)


Code Pipeling
As per AWS website: https://aws.amazon.com/codepipeline/pricing/

"With AWS CodePipeline, there are no upfront fees or commitments.

For V1 type pipelines: You pay $1.00 per active pipeline (a pipeline that has existed for more than 30 days and has at least one code change that runs through it during the month) per month. There is no charge for pipelines that have no new code changes running through them during the month. An active pipeline is not prorated for partial months. Pipelines are free for the first 30 days after creation.

For V2 type pipelines: You pay $0.002 per action execution minute. Action execution duration is calculated in minutes, from the time an action in your pipeline starts executing until that action reaches a completion state, rounded up to the nearest minute. You are charged for all action types except for manual approval and custom action types"

What's the Architecture design?
![image](https://github.com/user-attachments/assets/b6df7e4f-d9e0-4663-8039-90ba15a103d1)

Step 1- Fork the Repo and Configure S3 bucket

 We will be using Github as a way to fork the repository, which has been provided by our favorite tech gal, [Tiny Technical Tutoria](https://www.youtube.com/@TinyTechnicalTutorials)l! Be sure to fork the following git hub repo: https://github.com/CurlyKid/codepipeline-s3-game.git

Next, we will need to access the S3 empty bucket
-Enable static website under properties 
-Fill in "index.html" and "error.html" under index document and save permission
-Be sure to have "Block all public access" disable under permision
-Also paste the following policy into "Bucket Policy that's under the permission tab

Bucket Policy that will allow others to read the bucket contents be sure to paste your own bucket name for the arn
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::curlykidgameapp/*"
        }
    ]
}

Step 2- Enable Codepipe Line 

-Select codepipeline and create a new pipeline 
-Select supersede or Queued for the "Execution Mode"
-Select "New Service Role" 
-Hit next

For the Source provider, you can select from a variety of code repo's; in our case we will be using Github Verison 2
-Select Git hub
-Select "Connect to Github"
-Install new app to create connection
-Input information and click "Only select repositories" at the bottom
-Use the repository that's been forked previously and hit save
-For the next page, click connect

Now your pipeline Repository name should be available from your Github
-Next select "main" for the default branch
-Select "CodePipeline default" for the artifact format
-Select "No filter" for trigger type
-select next

Afterwards, you should see "Add build stage", which allows you to select CodeBuild or Jenkins as an option code building format. We will be covering this alterative at a later date, so for now click skip. 

Now you should be at the "Add Deploy Stage"
-Select "S3" for deploy provider 
-For Input artifacts select "SourceArtifacts
-Select the designated bucket created for this project 
-For Deployment path, click the "Extract file before deploy" box.
-Select Next 

Do a final review and click create pipeline at the bottom

The pipeline should be a success and you should see the source in progress while it finish connecting with GitHub

![Screenshot (17)](https://github.com/user-attachments/assets/932cd793-501e-43fc-979b-65b1815599b2)

Step 3- Testing our work
-Navigate back to S3 static website hosting 
-You should have a bucket end point that redirects you to the gaming app

![Screenshot (19)](https://github.com/user-attachments/assets/961de296-6831-455b-90c4-2cb837a34b83)


![Screenshot (20)](https://github.com/user-attachments/assets/573c1501-12e4-453c-9994-33a84676eda3)



Step 4( optional)- Here we will be using CodeBuild add new or custom code to our gaming app, adding some spice to the fun!
-Go to the GitHub repo that you've forked
-Open up the index.html page
-Click edit and on line 13 add "New Release"; "Welcome to the Meme Matching Game NEW RELEASE!"
-Click Commit changes and save

Now on codepipeline should display an update to the source code
![Screenshot (22)](https://github.com/user-attachments/assets/3c96fe81-a830-472c-901a-171374089930)

And our gaming app should show some updated text. 
![Screenshot (24)](https://github.com/user-attachments/assets/951829b8-0ba5-4125-a36d-1c20a4d0ad25)



And we are done! If you enjoy this project, feel free to share it with friends!
