
# Linux for Pentester: sed Privilege Escalation

sed --help

**![](https://1.bp.blogspot.com/-b4imRSdhFmM/XSgog1jdIWI/AAAAAAAAfKw/Q4AqvFRFR1gB8wy4X76sLBEiRR9DnBfdwCLcBGAs/s1600/1.png)**

**_Key actions achieved by “sed”_**

- **Replacement with the sed command**: As we know the “sed” performs many tasks that include insertion, deletion, modification and so on for any file as per user request so now we will start our journey to explore the entire utility of sed one by one.

1.1 _Substituting or switching string_: “sed” is used to replace or swap the string so whenever we need to exchange any string within a file then we will frame command as:

nano Ignite.txt
cat Ignite.txt
sed 's/Ignite/Egnyte/' Ignite.txt

In the above command “s” denotes the substitution action. The “_Ignite_” is the hunt pattern and the “_Egnyte_” is the replacement string. By default, the sed command replaces the first incidence of the pattern in each line and it won’t replace the second, third…occurrence in the line.

![](https://1.bp.blogspot.com/-JJn9Uv2kaAk/XSgojvOf-rI/AAAAAAAAfLM/0Psw4bQZNrkaMnBC0ohvqsHNdvUxhtWswCLcBGAs/s1600/2.png)

1.2 _Substituting the nth existence in a line_**:** When we want to replace nth occurrence i.e. first, second and so on the existence of a pattern in a line then we will use the /1, /2 etc flags to mention the nth term.

sed 's/Ignite/Egnyte/2' Ignite.txt

Here I’m swapping for 2nd occurrence in each line.

![](https://1.bp.blogspot.com/-1PYWINn6Fag/XSgojcU67OI/AAAAAAAAfLI/_3nX5PUg7AohcE6atFbrIu1pQ6xi-amOQCLcBGAs/s1600/3.png)

1.3 _Substituting_ _all the existence at a time:_ As we know by default the sed command replaces the first incidence of the pattern in each line so if we wish to replace all occurrence simultaneously within a file then we can use flag “/g” for this purpose.

 sed 's/Ignite/Egnyte/g' Ignite.txt

1.4 _Substituting from nth occurrence to all existences__:_ When we use “/g” this will make change globally to the entire file so if we want to make this swapping from a specific place then we need to mention that value(nth) from where we want to make changes.

sed 's/Ignite/Egnyte/3g' Ignite.txt

On framing the above command it will replace all the patterns from the nth occurrence globally.

**_Note_:** _In the below image you can’t see any changes for flag “3g” as my file doesn’t contain any 3rd occurrence of the replaced word but whenever there is the existence of substituted word at multiple times within a line then you can clearly see the changes that how its change globally from nth term._

![](https://1.bp.blogspot.com/-mzpBKRtF_Os/XSgoj_KjWrI/AAAAAAAAfLQ/QJTKUqiS0MsG98zd_qgsh0LiHJVCKHS7gCLcBGAs/s1600/4.png)

1.5 _Substituting the existence for a particular range:_  We can limit the sed command to replace the string for a particular range. This can be achieved by framing command as shown below.

sed ‘1,3 s/Ignite/Egnyte/’ Ignite.txt

On framing this command the “sed” will replace “Ignite” starting from the first line to the third line.

**_Note_**_:_  _One can use “$” in place of end index if we want substitute from nth term to the last line in the file._

![](https://1.bp.blogspot.com/-J772GSxeYPM/XSgokVUJKsI/AAAAAAAAfLU/MHnZappKJToSdv4BfCWF-_IBYxnNm6mjQCLcBGAs/s1600/5.png)

- **Printing and viewing from sed command_:_** Apart from substituting the string sed can help in printing and viewing a file as per user’s instruction.

2.1 _Replicating the replaced line with /p flag_: If we want to make duplication for replaced line then we can use the “/p” flag which prints the replaced line twice on the terminal. If a line does not have the search pattern and is not replaced, then it will print that line only once.

sed ‘s/Ignite/Egnyte/p’ Ignite.txt

2.2 _Printing only the replaced lines:_ If a user wants to print only those lines which are substituted then he can use “-n” option following by print command as shown below.

sed -n ‘s/Ignite/Egnyte/p’ Ignite.txt

As from below image it can be cleared that on using “-n” the print flag has printed all the replaced line as output.

![](https://1.bp.blogspot.com/-npMwgKEhChk/XSgok8Cv2RI/AAAAAAAAfLY/e_aw_vtVnyciN28Lb8q8c008-TDGvNTNACLcBGAs/s1600/6.png)

2.3 _Printing lines by numbering it__:_ This command is similar to “cat” in which we use “-n” for numbering the line for any file, same we can achieve from sed command too by framing the command as below.

sed = a.txt | sed 'N; s/^/     /; s/ *\(.\{4,\}\)\n/\1  /'

On drawing the above command sed will print the output by numbering each line as per user request.

![](https://1.bp.blogspot.com/-6lHtEHF_FAI/XSgolsKIfWI/AAAAAAAAfLc/rIWWhjqI-c45N_O67nTs_gzLXlnA7JO-gCLcBGAs/s1600/7.png)

2.4 _Display a file from x to y range:_ If we want to view a file from an instance i.e. for a range of starting index to end index then we write command as:

sed -n '2,4p' Ignite.txt

If we use “d” instead of “p” then sed will View the entire file except for the given range.

2.5 _Print nth line of the file_: Inplace of fixing end index you can also leave it blank if you wish to print only a specific line.

sed -n '4'p Ignite.txt

As in below screenshot, you can see when I have used above-mentioned command then sed has reflected the output only to print for the 4th line.

![](https://1.bp.blogspot.com/-vkkQwPnQLMQ/XSgoltfBMcI/AAAAAAAAfLg/dQTbJNgFzGwOrFpMzQU71L8wF26dCeuFwCLcBGAs/s1600/8.png)

2.6 _Print from nth line to end of file_: To print any file from its nth line to the last (end of file) line then frame command as below:

sed -n '4,$'p Ignite.txt

Here “$” is an indication for reflecting the last line of the file.

![](https://1.bp.blogspot.com/-Zl_mptXXrvA/XSgolx2_fRI/AAAAAAAAfLk/xy6JWLz-QMIe4WoSONfxLDZTnoNLXvnvwCLcBGAs/s1600/9.png)

2.7 _Print the line only for pattern matching_: If we want to print only those lines which match the given pattern then, in this case, we will draw command as:

sed -n /training/p Ignite.txt

From the below image, it is clear how this command works. Here in the below image, I have print those lines which include the word “training”.

2.8 _Print lines which matches the pattern nth line_: We can use numeric value along “p” to print for pattern matching till nth line.

sed -n '/cyber/,3p' Ignite.txt

![](https://1.bp.blogspot.com/-vz_hJ9qVGeY/XSgog6gIGnI/AAAAAAAAfK0/8kA3Fpw7nfsTmI83IUf-dqUUV441mRuwQCLcBGAs/s1600/10.png)

**3** **Deleting lines with sed****:** Now we check how we can delete the lines from a file by the help of sed.

 3.1 _Remove a specific line_: To delete any particular line within a file us “d” option followed by sed command. Here I’m deleting the 3rd line from “Ignite.txt”.

sed '3d' Ignite.txt

3.2 _Remove line for a range_: If we wish to delete content till a particular range then we will set its “initial index value” and “end value” of file. In below image, I have deleted the content of “Ignite.txt” from its 3rd line to 5th line and will attain output for remaining file content.

sed '3,5d' Ignite.txt

![](https://1.bp.blogspot.com/-J7n7yICEbFE/XSgogwvEq-I/AAAAAAAAfKs/-tvO3cp0ih0wlFDV6RizlPTSt7gPGvBKQCLcBGAs/s1600/11.png)

3.3 _Remove from nth to last line_: Instead of fixing end index one can also use “$” to delete lines till the end of the file.

sed '2,$d' Ignite.txt

Here “2” indicating for the initial index from where deletion must be done and “$” is indicating to delete lines till the end of the file.

3.4 _Remove the last line_: If we won’t set any index value then “$d” will simply delete only the last line of the file.

sed '2d' Ignite.txt

![](https://1.bp.blogspot.com/-NpFyt0o5s4Y/XSgoh3IJSCI/AAAAAAAAfK4/RVz3lcRiA8U7QHdIxx2uzzi3z5w6TdBswCLcBGAs/s1600/12.png)

3.5 _Remove the pattern matching line_: Sometimes we not only want to print or view those lines that match the particular pattern but also desire to delete them so in such case we will frame below command to attain output as per user request.

sed '/training/d' Ignite.txt

Here in below image sed has deleted all those lines which match the word “training”.

![](https://1.bp.blogspot.com/-h5JV4QfXhf0/XSgoiPXVZZI/AAAAAAAAfK8/o6NjrYaRtbwQhXB1aX1UC49d1ybMns_1QCLcBGAs/s1600/13.png)

### **Abusing sed**

### **Sudo Rights Lab setups for Privilege Escalation**

Now we will start our mission of privilege escalation. To grab this first, we have to set up our lab of sed command with administrative rights. After that, we will check for the sed command that what impact it has after getting sudo rights and how we can use it more for privilege escalation.

It can be clearly understood by the below image in which I have created a local user (test) who own all sudo rights as root and can achieve all task as admin.

To add sudo right open etc/sudoers file and type following as user Privilege specification.

test All=(root) NOPASSWD: /usr/bin/sed

![](https://1.bp.blogspot.com/-As3Os2l15Yo/XSgoiKBThlI/AAAAAAAAfLA/Yq_4htTJxQE-AxensdosWV33P5IpQ5UEACLcBGAs/s1600/14.png)

### **Exploiting Sudo rights**

Now we will start exploiting sed facility by taking the privilege of sudoer’s permission. For this very first we must have sessions of a victim’s machine then only we can execute this task. Suppose we got the sessions of victim’s machine that will assist us to have local user access of the targeted system through which we can escalate the root user rights.

So now we will connect to the target machine with ssh, therefore, type following command to get access through local user login.

ssh test@192.168.1.108

Then we look for sudo right of “test” user (if given) and found that user “test” can execute the pip command as “root” without a password.

sudo -l

Now we will access our /etc/passwd file by the help sed command to escalate or maintain access with elevated privileges.

**Conclusion:** Hence we have successfully exploited “sed” by achieving its functionality after granting higher privilege. 

![](https://1.bp.blogspot.com/-gpkPIn4iOSQ/XSgojOBQpBI/AAAAAAAAfLE/IuKz4o2iKg0cZ4lZv3Y2bsUSlFYAlTlEACLcBGAs/s1600/15.png)

Reference link: https://gtfobins.github.io

**Author**: Komal Singh is a Cyber Security Researcher and Technical Content Writer, she is completely enthusiastic pentester and Security Analyst at Ignite Technologies. Contact [**Here**](https://www.linkedin.com/in/komal-rajput-26b783131/)

### Related posts:

1. [File Transfer Cheatsheet: Windows and Linux](https://www.hackingarticles.in/file-transfer-cheatsheet-windows-and-linux/ "File Transfer Cheatsheet: Windows and Linux")
2. [OSINT : User Privacy in Linux](https://www.hackingarticles.in/osint-user-privacy-in-linux/ "OSINT : User Privacy in Linux")

## Post navigation

[Escalate_Linux: Vulnhub Walkthrough (Part 1)](https://www.hackingarticles.in/escalate_linux-vulnhub-walkthrough-part-1/)

[Matrix-3: Vulnhub Walkthrough](https://www.hackingarticles.in/matrix-3-vulnhub-walkthrough/)

### Leave a Reply

Your email address will not be published. Required fields are marked *

Comment *

Name 

Email 

Website 

 Save my name, email, and website in this browser for the next time I comment.

Search for: 

[![](https://www.hackingarticles.in/wp-content/uploads/2024/04/Courses.png)](http://bit.ly/ignitetechnologies)

[![](https://www.hackingarticles.in/wp-content/uploads/2024/04/Forensics.jpg)](https://www.hackingarticles.in/best-of-computer-forensics-tutorials/)

![](https://www.hackingarticles.in/wp-content/uploads/2024/04/MindMap-2.jpg)

[![](https://www.hackingarticles.in/wp-content/uploads/2024/04/Support.png)](https://www.paypal.com/paypalme/hackingarticles)

## Categories

Categories

[](https://www.hackingarticles.in/linux-for-pentester-sed-privilege-escalation/# "Go to Top")