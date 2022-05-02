---
layout: post
title: "On-Call: Leave It Better Than You Found It"
date: 2021-11-14
tags: devops helpdesk career
comments: true
---
For more than a year, I have been participating in on-call rotations as a Site Reliability Engineer at Vinted. 

During on-call, SRE handles all incidents, reacts to alerts, and responds to developer requests. It basically solves any issues that occur throughout the infrastructure. 

Here are the practical things I learned about the process.

## Prepare Your Tools
My first on-call felt too quiet. I wasn’t getting any alerts or requests. This was because my phone automatically went into "do not disturb" mode.

Before going on-call, enable notifications on your phone and remove the automatic "do not disturb" rules. Make sure you are always logged in to main services like GitHub, Slack, and monitoring tools. You don’t want to type your password and wait for 2FA provider notification during an incident. Test your mobile hotspot connection and always keep your laptop and charger nearby.

Having the tools required for the job seems like an obvious thing to do, but during on-call it's even more important.

## Improve Monitoring
If you see that some alert wakes you up from your sleep and it can wait, make sure to fix it, so it’s sent during business hours. Even better, downgrade the alert severity and reduce the total number of total alerts sent.  
For me, the best pull request is with more lines of code deleted than added. The same logic applies to alerting. Only by going on call will you learn which alerts are relevant and actionable.

Another important thing in monitoring is runbooks. If you get an alert and have no idea how to act on it, check if there's a runbook for it. If not, create one after you find the solution to the problem.

## Improve Infrastructure
Don’t limit yourself to monitoring, improve other parts of the infrastructure too! It’s good practice to patch things and automate them right away. It may be an alert about a server running out of storage or some other failing task.  

If you see that some service is crashing on a virtual machine, try to find the root cause. Did it start processing more data than before? Increase vCPUs.

Have you found a server with some old configuration option that causes it to crash? Reconfigure it right now instead of adding the task to the backlog.

For example, a server running Docker could alert that it’s running out of storage. Instead of running a prune command, create a cron job that does it for you on all instances of Docker. Even better, adjust the Docker configuration so it doesn’t leave dangling images and containers.

By constantly improving infrastructure, you'll make it better and learn new things.

## Write Post-Mortems
Write good and actionable post-mortems. They are intended to investigate what happened and what steps must be taken to ensure that it does not happen again. 

Make sure to find the exact root cause of the incident, research and test all scenarios, and provide concrete steps your team will take to solve the issue. 

Communicate that to the whole company, so they know your team is taking care of it. This will help build professionalism and trust in the company, so it's definitely worth it.

## Take Some Time to Learn
On-call is the only time you will connect to all those servers that you have no idea what they're doing. This is a good opportunity to learn about the infrastructure that you regularly don't deal with. 

Take some time to dig into how all the systems are configured. Read the official documentation. Read configuration playbooks and try to understand how they fit into the bigger picture. Check service discovery tools to understand what services your company is running. 

Invest the time during on-call because after it ends, you'll get back to your project work.

## Ask For Help
Don’t be afraid to ask your teammates for help. Nobody will be angry if you don’t know even the most trivial fix to the problem.  

During an incident, everyone wants one thing: to end the incident. 

It’s better that you try to contribute than be shy and keep the possible resolution to yourself. Your colleagues will be glad to help you if you don't understand something.

## Stay Calm
Production incidents are inevitable. They happen in all companies, and they will happen in your company as well. They will happen when you're on-call. Don’t panic and stay calm! 

I got "lucky" and we had a production incident that caused full downtime during my first on-call.

In short, here’s how it went (redacted for privacy):
- 19:44 - Alert from PagerDuty.
- 19:47 - I am trying to create incident channel. Slack doesn’t load. I close Slack, re-open it. I try to invite my teammates to incident channel, only half of them are actually invited. Slack crashes again. Finally I manage to invite all team members to incident channel.
- 19:48 - We begin digging into the issue.
- 20:13 - We find the root cause.
- 20:19 - We apply the first fix.
- 20:23 - First outage ends.
- 20:40 - Second wave of alerts from PagerDuty.
- 20:42 - We immediately notice the leftover issue and apply the second fix.
- 20:43 - For a short moment all portals are down until we switch traffic.
- 20:44 - Outage ends.

Those moments can be stressful, but with the right mindset they are just another part of your work.

## Conclusion
On-calls can be intimidating, but once you develop the habit of constantly improving everything, they can become something you wait for.

It helps a lot to know that your team is behind you and that nobody will judge you over the mistakes you make.
