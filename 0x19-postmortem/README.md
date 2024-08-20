# Postmortem: Login Service Outage

![Postmortem GIF](postmortem.gif)

## ISSUE SUMMARY

### Duration
The outage lasted from **August 14, 2024, 1:30 PM to August 14, 2024, 2:15 PM UTC (45 minutes)**.

### Impact
The main website's user login service was down, resulting in **70% of users unable to log in**, affecting both desktop and mobile platforms. Some users experienced page load time delays of up to **30 seconds**.

### Root Cause
A misconfigured Nginx server during a routine deployment caused the load balancer to fail under a sudden surge of traffic, leading to an outage of the login service.

---

## TIMELINE
...
