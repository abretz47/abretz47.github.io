[<--Back to Portfolio](https://github.com/abretz47/abretz47.github.io)

# Summary and Purpose
Below are summaries of tickets opened with BPLabs on behalf of <Redacted Client> that required unplanned BPM outages happening in Q4 of 2017. The date the ticket was opened, the ticket number, and ticket subject headline each summary. The full ticket details have been exported to a compressed folder and have been distributed to all parties in attendance for our 1/23/2018 meeting. 

## Ticket Details

### 10319: Users not in tw_allusers - 12/19/2017
BPM uses a grouping mechanism that is integrated with <Redacted Client>’s LDAP system. There is a root group known at tw_allusers that all other groups stem from. However, if the user does not exist in tw_allusers, it can cause tasks to fail when BPM tries to assign tasks to users who have been removed from the tw_allusers groups. We were able to install a fix from IBM to address users falling out of the tw_allusers group, but this did require a BPM restart.

### 10308: LDAP Connection Issues - 12/18/2017
A <Redacted Client> network issue was causing packet loss in some requests over the network. As a result, the connection to <Redacted Client> LDAP server was likely hindered. The side-affect that users were unable to choose users within BPM screens to assign subsequent tasks to. After the network was brought back to a stable condition, it was determined that BPM had a stale connection with the LDAP server and so a restart was required. Upon restarting the BPM application and tuning the connection cache, BPM side-affects noted above were resolved. 

### 10287: Production Issues-12/13/2017
<Redacted Client> network issue showed side-affects in BPM users being able to connect to Sharepoint and search for users to assign tasks to. This eventually had a trickle down to Brazos Portal, where blocked thread caused by network latency starved the resources of the BPM server. A Brazos Portal restart was performed. This began starving the resources on the server causing other application misbehavior. The BPM server returned to a steady state after a Brazos Portal restart released the blocked threads. 

### 10227: Restart IHS and BPM server today (12/1/17) at 5:30 - 12/1/2017
It was realized that our SSL certificates were expiring soon. A <Redacted Client> IT server admin was needed to generate the new certificates from each server. After the SSL certificates were installed, we needed to restart the web server (not BPM server) that sits in front of the BPM server. This is known as the IBM HTTP Server, or IHS. It was noticed by <Redacted Name 1> (<Redacted Client> IT) that the BPM server was behaving less than optimally (high CPU, slow response running shell commands), so it was decided we would restart the BPM server. It was noted that the applications consuming the most resources on this sever did not relate to BPM and we never heard of any reports of application misbehavior prior to this restart. This CPU spike appears entirely unrelated to those of #10021, #9899, and #7778. Nevertheless, we restarted the server hosting BPM in production in order to kill the applications consuming resources. The discovery and resolution of this issue happened after hours. 

### 10021: 100% CPU usage and hung tasks on BPM server - 10/24/2017
A CPU spike with locked event manager was detected and required an immediate restart. Users normally find that the system is very unresponsive during these kind of outages and instances appear to get “stuck” (tasks not completing or generating the next task). BPLabs was already investigating a similar issue that was reported on 10/01/17 (#9899) and 11/26/17 (#7778). This occurrence, as well as #9899 and #7778 all showed the same negative behavior from BPM. Their stack traces matched at the points of failure within BPM. In the case of #10021, it was a blocked database transaction that caused a cascade of errors in BPM. We cannot conclusively say at this point what caused the transaction in the database to be blocked, but we have enabled Event Manager Task History logging (not normally necessary, extra system maintenance required) to get a clear picture of the events leading up to this cascade of errors that results in a stuck Event Manager and a CPU spike to the BPM server. 

### 9899: CPU on server hosting BPM is at 100% - 10/02/2017
Early on 10/02, it was noticed by <Redacted Client> IT monitors that the BPM server CPU usage was critically high. When BPLaps looked deeper, it appeared that the Event Manager had locked up, which caused the symptoms described above in #10021. As a result, an immediate BPM restart was performed to remedy any issues seen by users. Another case was opened with BPLabs to determine the root cause, which is detailed further in #10021. 

[<--Back to Portfolio](https://github.com/abretz47/abretz47.github.io)
