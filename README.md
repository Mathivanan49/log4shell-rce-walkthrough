# log4shell-rce-walkthrough
## What is Log4Shell?
Log4Shell is a critical vulnerability in Log4j that allows remote code execution.

Task 1:

Start the machine

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/c46a92d4d290e6609110819340b96546ae44038b/Screenshot%202026-03-20%20135139.png)
Task 2:

Perform the nmap service version detection scan to see the service running

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/059bb05fea85eb589c171382b645b63941f64079/Screenshot%202026-03-20%20112148.png)

From the result the answer will be apache solr

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/c3bd27ce9fb33d11b10b93311ccb2dadc656ab97/Screenshot%202026-03-20%20140826.png)

Task 3:

Visit http://MACHINE_IP:8983 and see the front page to find out log4j used








