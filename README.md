The Entire Assignment has been done through (AWS EC2 instance Centos-7)


1.cloned the csvserver image from the dockerhub repository ------------> docker pull infracloudio/csvserver:latest

2.tried creating a container from the cloned image,but found an Error(error while reading the file "/csvserver/inputdata": open /csvserver/inputdata: no such file or directory) -----> docker exec -it 8cb989ef80b5 --bash

3.Reason-There was not inputdatafile(volume) attached to the container csvserver

4.created a bash file gencsv.sh with executable rights to it ------> touch gencsv.sh / chmod a+x gencsv.sh

5.Created a script for random values with index numbers ------------------>
#!/bin/bash
count=1
while [ $count -lt 11 ]
do
rand=$((RANDOM%100))
echo "$count, $rand" >> inputFile
count=$(($count+1))
done 

6.Executed the bash file--------------------> ./gencsv.sh

7.Output was an inputFile 

8.attached/mounted the input file to the image  infracloudio/csvserver ---------------> docker run -it -v /opt/csvserver/inputFile:/csvserver/inputdata infracloudio/csvserver:latest

9.Ran the container with the resulted container id ------------> docker exec -it 9232365931f bash

10.searched for the port status of the container wherein i found the container was exposed to (port 9300)---------> netstat -an

11.Now Exposed the container port 9300 to host port 9393 ----------> docker run -it -v /opt/csvserver/inputFile:/csvserver/inputdata -p 9393:9300 infracloudio/csvserver:latest

12.now specified environment variable CSVSERVER_BORDER with value Orange ----------> docker run -it -v /opt/csvserver/inputFile:/csvserver/inputdata -p 9393:9300 -e CSVSERVER_BORDER='Orange' infracloudio/csvserver:latest
