# Gitlab_Runner_Disk_Reduce
Gitlab_Runner_Disk_Reduce

https://unix.stackexchange.com/questions/125429/tracking-down-where-disk-space-has-gone-on-linux


1) If you see any red runners when you run "sudo gitlab-runner verify", then we need to delete them and clean them. To delete them go to /etc/.gitlab-runner directory and open config.toml file and find the red IDs then delete them from the file and save the file.



2) Normalde 1 haftadan eski docker image larını silen cronjob yazmıştık. Ancak disk space sorunu olduğu için (df -h) 6 ve 7 günlük image ları da şu aşağıdaki komutla sildik ;

docker image rm 0b9ddcb8259e
docker image rm 16f6b40d1b0e --force 

3) 
docker ps -a -q -f status=exited | xargs --no-run-if-empty docker rm -v    # Remove exited processes   
docker volume ls -qf dangling=true | xargs --no-run-if-empty docker volume rm  # Remove dangling volumes   



***  NOTES FROM SADDAM 


/dev/nvme0n1p1   50G   42G  8,5G  84%


this should not be the issue, but it might affect us


could you try deleting all images that are older than a week please

You mean images in AWS ECR ?

nope, images on Docker of the gitlab runner


could you check docker logs as well please

could you also run the runners validate command

sudo gitlab-runner verify

If you see any red runners, then we need to delete them and clean them

I think our issue is related to Docker itself

as it is very slow

Some docker images could not be deleted. Then Saddam said "I think those images are used by Docker to build the projects"

therefore, you would not be able to delete them

we should focus on deleting our repos images that were already deployed

Then Saddam also said we should find what is eating the space.




---------------
We did not run the commands below not to cause any issues ;

# Remove dangling imagesdocker images -f "dangling=true" -q | xargs --no-run-if-empty docker rmi
# Remove unused imagesdocker images | awk '/ago/  { print $3}' | xargs --no-run-if-empty docker rmi







