---
template: blog-post
title: Google Cloud certification proctored exam review and tips
slug: proctored-google-cloud-exam
date: 2023-03-14 22:44
description: "This post provides some recommendations for taking the Google
  Cloud Associate Cloud Engineer exam and presents some details of the online
  proctored exam. "
featuredImage: /assets/mitchell-luo-xqjovx-x5-k-unsplash.jpg
---
## Introduction

We are migrating to Google Cloud at work and there has been encouragement to get certified. I had prior experience with both AWS and its (onsite) certification exams, but this was the first time I was exposed to Google Cloud. 

I will try to walk you through the straightforward preparation process I followed, and I will make some recommendations on A Cloud Guru courses that you could take for different levels of Google Cloud exams. Finally, I will try to give you an idea of how the online proctored exam was administered - there were some noteworthy nuances for me.  

My motivation with this  blog post is to answer some questions on these topics in hopes of helping my fellow developers in taking the related exams. 

## About Google Cloud certification exams

You can find the overview of the available exams [here](https://cloud.google.com/certification).  As it is evident on that page, there are three levels: 

* Foundational certification
* Associate certification 
* Professional certification 

I took the associate level exam, but will try to recommend the courses on the other levels too. 

## Learning platforms

I was a long time fan of Linux Academy (they were headquartered right here in the KC Metro). Using its content a few years ago made my AWS exam a breeze. Since then this company was acquired by [A Cloud Guru](https://acloudguru.com/) and now the latter is part of Pluralsight? Hard to follow M&A these days… Either way, I was told that the A Cloud Guru courses (with labs) are now available on Pluralsight as well, so you might be able to locate the courses I list below, on that platform. 

Originally, the aspect of Linux Academy that attracted me the most was the availability of lab exercises in the environments that were provisioned on-demand. Compared to dry video and text courses (accompanied by constant yawning), those labs made a huge difference in my understanding of the covered material and solidifying it. 

These platforms are expensive, but I am one of the lucky ones as my employer provided a license for A Cloud Guru. I set up a learning path for myself, which is really an ordered collection of courses you can take to achieve certain goals, such as certification. If you cannot have your employer pay for these platforms, one approach I recommend is signing up for a limited time, like 1 month and utilizing the platform frequently, like daily or every other day. Dedicating 1-2 hours per session, you can cover so much in a month and then you can cancel your membership. For about $50 per month, that will end up being a sweet deal. 

## Overview of A Cloud Guru courses focused on Google Cloud certifications

You can just search on the Cloud Guru platform by title to locate these courses. 

### General introduction

#### Google Cloud Essentials by Joseph Lowery (~12 hours)

I have taken this one. This is a more basic course but a great introduction. It has an amazing Lucid charts interactive diagram you can reference async, which kind of replaces the need to take your own notes (I still take my own notes).  There is an abundance of lab exercises.  

### Level 1 - Foundational certification

#### Google Cloud Certified Cloud Digital Leader by Joseph Lowery (~20 hours)

I have not taken this one.  It came up while researching the courses for a colleague. I am a big fan of this instructor and will post another course by him down below. 

### Level 2 - Associate certification level

#### Google Certified Associate Cloud Engineer by Broadus Palmer  (~17 hours)

I have taken this one. Very structured and organized. Plenty of lab exercises and 3 practice exams! This one is not  intended to include IAM concepts. 

#### Google Cloud Identify and Access Management (IAM) Deep Dive by Matthew Ulasien (~8 hours)

I have taken this one.  Absolutely recommend it if you are planning to take the Associate Cloud Engineer exam. To me it seemed like Google Cloud IAM slightly differed from AWS IAM. 

### Level 3 - Professional certification

At this level, unlike the previous levels, there is some branching based on the speciality of your choice. I am leanings towards Architect or Developer at this time. 

#### Google Certified Professional Cloud Architect by Joseph Lowery (~29 hours)

I have not taken this one - not there yet. But I just ended up being a big fan of Joe Lowery and his teaching methodology. So I look forward to taking this course when I am ready. 

### Preparation process

I basically took some of those courses above and watched more video courses recommended by the platform. Then I took the relevant practice tests and took the actual exam as soon as I got 80-90% of the practice tests right. 

# Overview of the online proctored exam

Here is some registration and scheduling info from Google: [Registration and Scheduling - Cloud Certification Help](https://support.google.com/cloud-certification/answer/9907651?hl=en). Seems like Google outsources these exams to a company called Kryterion. 

#### Before the exam

* It varies by the exam, but if needed, you should be able to reschedule your exam with a proper notice. It was 24 hours for online proctored exams for the Associate Cloud Engineer.
* Make sure you install their secure browser way ahead of the exam.
* It is possible you will be asked to enter biometric information in Webassessor, such as taking your photo through the webcam with white background. I tried to do that but I was asked to go through that again at the time of the exam. 
* It is recommended that you take the exam on your personal laptop or the one you have admin privileges for. Most work computers do not meet this requirement.
* The secure browser has the ability to detect the processes running in the background and will shut them down. I would just close out of everything that you think could be running on your machine. 
* You will not be able to find the `Launch` button for the exam until 10 minutes before the exam (in Webassessor). So just sit back and relax. 

### During the online proctored exam

* You are not allowed to get any support: digital/hard copy resources, another human being etc. 
* I did not even use my regular work from home desk. I just set up a plastic picnic desk in the middle of the room away from the walls, shelves etc.
* Not allowed to take breaks and cannot have a water bottle on the desk. I was asked to put away my driver's license outside of my reach after showing it to the camera. 
* The webcam is on for the entire duration of the exam. 
* I think there was a human proctor on the other side, but I did not hear or see the proctor. We communicated through the chat window the entire time but it was very interactive. 
* I was asked to capture each wall in the room for about 3 seconds and also the desk I am sitting at to ensure nothing extra was there - I just had a mouse next to my laptop. 

### Exam contents - I believe this applies to both associate and professional exams

* 50 multiple choice questions. You can mark them for review and come back to them later. 
* Y﻿ou do not have to get all 50 questions right. I think getting 70% right is sufficient, but I could not find a Google Cloud source to confirm that. 
* I﻿f the initial questions are hard, just move on the next ones and come back to the hard ones.
* The duration is 2 hours but you can finish early. 
* A Cloud Guru did a good job of imitating the real exam in the practice ones. 

### After the exam

* Kryterion gives a PASS/FAIL result right away. 
* I was told my file would be sent to Google for final review and Google would reach out with the official outcome.
* I got an email from Google in 2 days after the exam. Too bad there was no free swag... But I heard Pro level exams come with swag.

I﻿ hope this info helps and good luck in your cloud certification exams! All opinions are my own and also keep in mind, this ecosystem keeps constantly changing so both Google Cloud exams and the proctoring details can change rapidly. 

I﻿n conclusion, (associate level) certifications are good in getting comprehensive surface-level understanding of the topics. But really, nothing replaces real, hands-on, production-level experience you gain while implementing certain things in the cloud environments.