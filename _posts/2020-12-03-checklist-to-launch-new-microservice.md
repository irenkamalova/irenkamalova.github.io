
---  
categories: article  
---  
![Image for post](https://miro.medium.com/max/1219/0*MiT2LhRLDwmp4Q9x)

[**Photo by Glenn Carstens-Peters,**](https://unsplash.com/photos/RLw-UC03Gwc) [**SpaceX**](https://unsplash.com/photos/OHOU-5UVIYQ) [**on Unsplash**](https://unsplash.com/photos/RLw-UC03Gwc)
  
Nowadays, microservice architecture is very popular. If you want to add new functionality and you are willing to maintain the infrastructure for this component as a self-isolated microservice — even if it’s not the first microservice in your system, you have some steps to do. it’s better to think about it in advance, otherwise you might block DEV and QA teams! So, don’t put this routing at the end, take care of your development process at the start! Here’s a checklist of steps that should be done.
   
> A checklist is a type of job aid used to reduce failure by compensating   
  for potential limits of human memory and attention.   
  It helps to ensure consistency and completeness in carrying out a task.  
> -- <cite>Wikipedia</cite>dit.io/).

**1. Scripts to wrap microservice to the executable component**

Here  you need to ask the following questions: should you build an image from a docker file or will it be an executable jar? Which script should be defined to build it? Where will it be?

**2. CI/CD pipeline**

Do you use Jenkins or Bamboo, or anything else for your CI/CD? Create jobs that build a new version of microservices.

**3. Create jobs that run tests**

![Image for post](https://miro.medium.com/max/898/1*VkyECAfcc9olrvesbqlA2w.png)

Green tests!

There should be one job for unit tests and the one for integration tests. The one for unit tests should be built on every pull request to prevent from merging untested code. The one for integration tests should run every night and check only one “develop” branch. That’s advice for saving resources in your CI/CD environment.

**4. Properly define resources for your integration tests**


![Image for post](https://miro.medium.com/max/1250/0*U5g-d01rjnWemnS-)

[**Photo by Zoe Schaeffer on Unsplash**](https://unsplash.com/photos/H0iAXFekiWo)

You have three possibilities for these tests:

-   Service connects to UAT environment

In this case, you should define access from CI/CD environment to UAT. Also, you should be able to support requirable services on UAT for the whole time. (constantly) If you don’t have a prod environment, that case probably is not for you.

-   Service connects to containers in the CI/CD environment.

For example, every job on the start of launching requirable services from docker files in containers. Your service has access to them on even the local host.

-   Service use test framework docker container.

Probably, the most comfortable way for development: integration tests can be easily maintained in a  _dev_ environment. But you need 1) a possibility from your CI/CD environment for downloading your test’s docker images; 2) they shouldn’t interfere with the existing CI/CD environment.

**5. Link your repository to the CI/CD environment**

This step shouldn’t be missed. Set rules which allow you to merge safely into your main branch.


![Image for post](https://miro.medium.com/max/1250/0*DwU4ANCPGFZulBNe)

[**Photo by Toa Heftiba on Unsplash**](https://unsplash.com/photos/_UIVmIBB3JU)

**6. Provide automated deployment to your environment**

This step is important. Let your developers only merge new code and see results in the  _dev_ environment without any other manipulations!

**7. Create jobs which deploy service to different environments**

-   The one for the UAT environment
-   The one for the PROD environment
-   For the ideal case — the one for the DEV environment…
-   For the perfect case — the one for the PRE-PROD/STAGE environment!

**8. Scalability of service**

Once you deployed your microservice, think about how you’re going to support a high rate of the  [RPS](https://en.wikipedia.org/wiki/Queries_per_second#:~:text=Queries%20per%20second%20(QPS)%20is,requests%20per%20second%20(RPS).)  level and load balancing? Should there be several services or should there be a property to execute several threads? How does it affect resources in your system? Do you have enough CPU and memory? Put notes about how much this service is consuming for now.

**9. Authorization and authentication (optional)**

If you don’t have any new restrictions on your component you can skip this step. Otherwise, you should answer several questions and prepare all work that needs to be done. Should your application have certs keys? Should it provide a login and password for accessing external services? Should it store ssh-keys inside?

**10. Secrets (optional)**

![Image for post](https://miro.medium.com/max/1250/0*t7Hjc445Vi06G-q3)

[**Photo by Kristina Flour on Unsplash**](https://unsplash.com/photos/BcjdbyKWquw)

If you have some sensitive information in your application like passwords, tokens, OAuth, and ssh-keys — don’t forget to create or update secrets for it.

Microservices architecture should be supported by a lot of tasks on infrastructure management, but you need to consider using this architecture if you don’t. Check here  [why](https://oleg-stadnichenko.medium.com/why-should-you-use-microservices-49187839142d)! We always want to skip some routine tasks like linking SVN with CI/CD, but this job is something that should be done at the start of working with a project. The good todo-list helps you simpler schedule all work to be done instead of postponing these tasks to the last moment! Be calm, be insured, drink coffee, and make your environment great again!


![Image for post](https://miro.medium.com/max/1250/0*UtI-VS2ZBBlKNdBE)

[**Photo by Prateek Katyal on Unsplash**](https://unsplash.com/photos/FcdtuGf7TEc)