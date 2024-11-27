# AIX Workshop
## Create PowerVS AIX instance
1. Use IBM cloud portal to create AIX 7.2 instance
2. Login to instance 
   
   `ssh -i ~/.ssh/<keyname> root@x.x.x.x`
3. set system wide path for AIX toolkit 
   `vi /etc/environment`
4. Find path line and add following in the end
   `:/opt/freeware/bin`
5. To save and quit the vi, press escape and type following and press enter
   `:wq`
6. Exit and start the ssh session again to reload the environment file.
7. Now run following command to test path is set, you should see :/opt/freeware/bin in the end 

   `echo $PATH`


## Increase size of opt
1. From IBM cloud portal update the boot disk size.
2. Once the boot disk is updated please follow next steps in shell after connecting to server
3. Run following command to rescan the disk 
   
   `cfgmgr`
4. Check current physical partitions in rootvg, and look for filed "FREE PPs"
   
   `lsvg rootvg`

5. Resize the physical volume to recognize the new size 
   
   `chvg -g rootvg`

6. verify updated rootvg size
   
   `lsvg rootvg`

7. Identify the logical volume to expand, in this case we will look for volume where /opt is mounted

   `lsvg -l rootvg`

8. Extend the logical volume. Number of PPs can be identified by dividing "to be size in GB"/ PP SIZE. Then look at the currrent PPs assigned to /opt and subtract this from the Number of Identified PPs
   
   `extendlv <lvname> <num_of_pps>`
9. Now before we install any additonal software increase the size of /opt by running following command 
   
   `chfs -a size=10G /opt`
   
## Run nsum
1. Install git 
   
   `dnf install git`

2. Clone nusm repo
   
   `git clone https://github.com/nigelargriffiths/nsum.git`

3. Make file executable  
   
   `chmod -R +x /nsum`

4. Now add nsum to path
   
   `export PATH=$PATH:/nsum/`

5. Now cd into directory where nomon files are located and run following commands:
   
   `nsum_run`

