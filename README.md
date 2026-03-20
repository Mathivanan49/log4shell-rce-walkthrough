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

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/1857219ebc47ef2a4bcf0ec4e996463dec8bbf8c/Screenshot%202026-03-20%20112830.png)

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/914646309bf26f8f955b7692102a12c0b21ee6d8/Screenshot%202026-03-20%20112840.png)

The solr.log is the file that has significan info downloaded from the task file

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/bf697c932cc79b2f8643370c2bb00b5be59fc876/Screenshot%202026-03-20%20113056.png)

The URL endpoint is indicated in these repeated entries are

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/e38c9683d7c0da229d269b74b8fef98332c3f9e9/Screenshot%202026-03-20%20114610.png)











