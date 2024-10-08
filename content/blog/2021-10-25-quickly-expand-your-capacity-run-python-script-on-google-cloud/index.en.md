---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Quickly expand your capacity: 
  run Python script on Google Cloud"
subtitle: ""
summary: ""
authors: [Lukas Wallrich]
tags: []
categories: [coding]
date: 2021-10-25T11:35:32+01:00
lastmod: 2021-10-25T11:35:32+01:00
featured: false
draft: false
slug:  quickly-expand-your-capacity-run-python-script-on-google-cloud

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: "Cloud computing can save a lot of time - if the set-up is quick"
  focal_point: "TopRight"
  preview_only: false

projects: []
---


I am currently working on some simulation studies using agent-based models, which can quickly take a very long time to run. Rather than having my system slowed for hours/days, I wanted to find a way to *quickly send code into the cloud.* This seemed to be a very obvious use of cloud computing, but proved a little less straight-forward than expected. However, some existing solutions got me 80% there — but they still require a fair bit of manual tuning for every script, and as always, the last 20% ended up consuming a fair bit of time. That’s why I want to share a nearly out-of-the-box solution here.

# Preview — or what pyscript2gce can do

Once the workflow is set up, it takes four steps to get your script to run on Google Cloud and to get your results:

1. Paste your code into the template and add commands to save the outputs (and to send email updates, if you like)
2. Push your code to GitHub (which can be one line if you set up a git lazy shortcut), then wait a moment for Docker image to get built
3. Create the virtual machine that runs your code (with a simple command-line command), then wait for the email that tells you that it is done
4. Download the output from Google Cloud Storage (again a one-line command)

# How to get there

To get this workflow going, you need to [fork the Github repo](https://github.com/LukasWallrich/pyscript2gce-template), sign-up to Google Cloud (if you haven’t done so; note that there are fairly generous free trials available), and then follow the step-by-step guide in the Github README.

**NB:** This is work-in-progress, and highly customisable. I hope it works for you - let me know if you have feedback.