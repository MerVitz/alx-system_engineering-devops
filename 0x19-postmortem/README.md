# Postmortem: Login Service Outage

![Postmortem GIF](./postmortem.gif)

## ISSUE SUMMARY

### Duration
The outage lasted from **August 14, 2024, 1:30 PM to August 14, 2024, 2:15 PM UTC (45 minutes)**.

### Impact
The main website's user login service was down, resulting in **70% of users unable to log in**, affecting both desktop and mobile platforms. Some users experienced page load time delays of up to **30 seconds**.

### Root Cause
A misconfigured Nginx server during a routine deployment caused the load balancer to fail under a sudden surge of traffic, leading to an outage of the login service.

---

## TIMELINE

- **1:30 PM**: Monitoring alert triggered for increased HTTP 502 error rates on the login service.
- **1:32 PM**: An engineer on-call received the alert and began investigating the issue.
- **1:35 PM**: Assumed root cause was a DNS misconfiguration due to recent domain changes, but this was ruled out after a DNS check.
- **1:45 PM**: Misleading debugging occurred, leading the team to believe there was an issue with the application code itself.
- **1:50 PM**: Incident was escalated to the DevOps team for deeper analysis of the server infrastructure.
- **2:00 PM**: The DevOps team identified the issue as a misconfigured Nginx load balancer that was not correctly distributing traffic.
- **2:05 PM**: Nginx configuration was corrected, and additional instances were spun up to handle the traffic spike.
- **2:15 PM**: All services were restored to full functionality, and monitoring showed the issue was resolved.

---

## ROOT CAUSE AND RESOLUTION

### Root Cause
During a routine update of the Nginx server configuration, an incorrect directive was added that limited the number of active connections that the load balancer could handle. This caused the server to become overwhelmed during a traffic surge, resulting in **HTTP 502 errors** for users attempting to log in.

### Resolution
Once the misconfiguration was identified, the team immediately rolled back to the previous Nginx configuration. In addition, the team deployed **two additional instances** to distribute the traffic more effectively. After verifying that the load balancer was functioning correctly and traffic levels normalized, the system was brought back online.

---

## CORRECTIVE AND PREVENTATIVE MEASURES

### Improvements
- Implement better change management processes, including automated testing of configurations before deployment.
- Introduce autoscaling for the Nginx servers to handle unexpected traffic spikes without manual intervention.
- Increase the level of logging and monitoring on critical infrastructure components.

### TO DO
1. Patch the Nginx server to include automated testing on deployment.
2. Add monitoring for server memory and CPU usage.
3. Set up autoscaling policies for Nginx load balancers.
4. Conduct a post-incident review with the development and DevOps teams to refine processes.
