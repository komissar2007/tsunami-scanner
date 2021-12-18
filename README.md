# tsunami-scanner
## overview:
- tsunami scan will run via jenkins pipeline, it will use slavarub/tsunami docker image which will get ip address as input ,
process scan and publish its results
- in case of failure , jenkins job will marked as failed and will publish message in the log

- create new pipelinejob in jenkins which will use Jenkinsfile from current repository (must run on linux machine with docker access)
![input](https://user-images.githubusercontent.com/2504356/146637734-51d3da5a-2e8f-45b3-8fdd-e1e10d339476.png)
- click on build and fill input with Ip list (comma seperated)
- process will run by run scanning from slavarub/tsunami docker image
![output](https://user-images.githubusercontent.com/2504356/146637739-b8b46486-90fa-46d7-a583-5e9e9a741bb1.png)
- report of each scan will be available on job artifacts

![report](https://user-images.githubusercontent.com/2504356/146637719-769be724-f6d6-4842-8fe2-7013174931f8.png)
