---  
categories: article  
---  
![Image for post](https://miro.medium.com/max/1219/0*MiT2LhRLDwmp4Q9x)  
  
[**Photo by Glenn Carstens-Peters,**](https://unsplash.com/photos/RLw-UC03Gwc) [**SpaceX**](https://unsplash.com/photos/OHOU-5UVIYQ) [**on Unsplash**](https://unsplash.com/photos/RLw-UC03Gwc)  
    
Nowadays, microservice architecture is very popular. Suppose you want to add new functionality or features to your services. If you have the time and hardware to invest in the maintenance of the infrastructure for a self-isolated microservice, then you have some steps to take. Even if this is not the first microservice in your system, there is still a lot of work to be done.

It’s better to plan these tasks in advance, otherwise you might block DEV and QA teams! So, don’t make these steps a low priority; take care of your development process at the start!
     
> “Why do I need to define a checklist for this process?”
> A checklist is a type of job aid used to reduce failure by compensating for potential limits of human memory and attention. It helps to ensure consistency and completeness in carrying out a task.
> -- <cite>Wikipedia</cite>.  

Here’s a checklist of steps that should be done…
  
**1. Create scripts to wrap microservice to executable components**  
  
Here you need to ask the following questions: should you build an image from a docker file or will it be an executable jar? Which type of configuration should be defined to build it? Where will it be stored and executed?
  
**2. Establish the CI/CD pipeline**  
  
Do you use Jenkins, Bamboo or something else for your CI/CD? Define jobs that build a new version of existing microservices and re-deploy them.
  
**3. Set up jobs that run tests**  
  
![Image for post](https://miro.medium.com/max/898/1*VkyECAfcc9olrvesbqlA2w.png)  
  
Green tests!  
  
There should be one job for unit tests and the one for integration tests. The job for unit tests should be executed upon every pull request, to prevent from merging untested code. The job for integration tests should run every night. Each integration test should check only one “develop” branch; do not make the error of overloading your system by running checks on multiple branches! This advice will save valuable resources in your CI/CD environment. 
  
**4.  Properly define resources for your integration tests**  
  
  
![Image for post](https://miro.medium.com/max/1250/0*U5g-d01rjnWemnS-)  
  
[**Photo by Zoe Schaeffer on Unsplash**](https://unsplash.com/photos/H0iAXFekiWo)  
  

You have three possibilities for these tests:

-   Service connects to ‘user acceptance testing’ (UAT) environment.
    

In this case you should define access from CI/CD environment to UAT. Also, you should be able to support required services on UAT consistently throughout the testing cycle. In the case that you don’t have a production (PROD) environment, this option is probably not for you.

-   Service connects to containers in the CI/CD environment.
    

In this case, the application is launched from "DockerContainer". Your service has access to these containers on the local host.

-   Service uses test framework docker container.
    

This is probably the most comfortable method for development. Integration tests can be easily maintained in a development (DEV)  environment.

However, you need:

1.  Possibility for your CI/CD environment to download your test’s docker images.
    
2.  Test containers shouldn’t interfere with the existing CI/CD environment.
  
**5. Link your repository to the CI/CD environment**  
  
This step shouldn’t be missed. Set rules which allow you to merge safely into your main branch.  
  
  
![Image for post](https://miro.medium.com/max/1250/0*DwU4ANCPGFZulBNe)  
  
[**Photo by Toa Heftiba on Unsplash**](https://unsplash.com/photos/_UIVmIBB3JU)  
  
**6. Provide automated deployment to your environment**  
  
Create the continuous deployment of new code by running configuration management or infrastructure as a code (IaC). In an ideal scenario, developers would focus solely on creating new code and thereafter be able to merge and test it, without any redundant actions. 
  
**7. Create jobs which deploy services to different environments**  
  
-   A job for UAT environment
-   A job for PROD environment
-   In an ideal scenario  —  a job for DEV environment…
-   In a perfect scenario  —  a job for PRE-PROD/STAGE environment!
  
**8. Scalability of services**  
  
Once you deployed your microservice, think about how you’re going to support a high rate of the  [RPS](https://en.wikipedia.org/wiki/Queries_per_second#:~:text=Queries%20per%20second%20(QPS)%20is,requests%20per%20second%20(RPS).)  level and load balancing? Should there be several services? Should the service have the property of being able to execute several threads? How does scaling affect resources in your system? Do you have enough CPU and memory? Record notes about how much this service is consuming currently and as it scales.
  
**9. Authorization and authentication (optional)**  
  
If you don’t have any access restrictions on your component you can skip this step.

Otherwise, you should consider several questions and prepare all work that needs to be done in order to secure the micro-service. Should your application have certification keys? Should it provide a login and password for accessing external services? Should it store ssh-keys inside?

  
**10. Secrets (optional)**  
  
![Image for post](https://miro.medium.com/max/1250/0*t7Hjc445Vi06G-q3)  
  
[**Photo by Kristina Flour on Unsplash**](https://unsplash.com/photos/BcjdbyKWquw)  
  
If you have some sensitive information in your application like passwords, tokens, OAuth, and ssh-keys — don’t forget to create or update secrets for it.  
  
Microservices architecture should be supported by a lot of tasks on infrastructure management, but you need to consider using this architecture if you don’t. Check here  [why](https://oleg-stadnichenko.medium.com/why-should-you-use-microservices-49187839142d)! We always want to skip some routine tasks like linking SVN with CI/CD, but this job is something that should be done  at the beginning of a project. A to-do list like this will help you schedule all work to be done, instead of postponing these tasks to the last moment! Be calm, be reassured, drink coffee, and make your environment great again!  
  
![Image for post](https://miro.medium.com/max/1250/0*UtI-VS2ZBBlKNdBE)  
  
[**Photo by Prateek Katyal on Unsplash**](https://unsplash.com/photos/FcdtuGf7TEc)