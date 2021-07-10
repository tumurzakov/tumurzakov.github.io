Recently on one of my side projects the time come to start MVP to prove hipothesis.
I need a solution to fast start without own ifrastructure and decided deploy project to cloud.
Using kubernetes on daily basis made me too lazy to use ordinary virtual machines so I glanced at managed kubernetes services.

There are several competitors:
* GKE with 300$ for 90 days (0.1$ per hour ~ 75$ per month)
* Azure with 200$ for 30 days (0.1$ per hour ~75$ per month)
* DigitalOcean for 200$ for 60 days (40$ for 2nodes per month)
* IBM for 200$ for 60days (0.29$ per hour ~ 208$ per month 😞)
* Oracle for 300$ for 30 days (~77$ per month)
* Alibaba ? for free (64.8$ per month)

I started with GKE and after credit come to end I'll switch to DO and then to Azure, so I will have at least half a year of free usage before I'll prove or fail with idea. Ideal conditions.

# GKE bootstrap

1. Creating new google account and attaching newly issued card
2. Activate GKE account
3. Go to Kubernetes Engine section in menu
4. Creating cluster and choosing autopilot option
5. Downloading gcloud console utility
6. Following steps on [github manual](https://docs.github.com/en/actions/guides/deploying-to-google-kubernetes-engine) to use in actions
7. Creating new kubectl context with `kubectl config use-context humanway`
8. Fetching GKE credentials with `gcloud container clusters get-credentials humanway-1 --project <project> --region <region>`, where project and region could be found on dashboard page of kubernetes cluster in GKE management page.
9. Thats all, now you can deploy to k8s with helm

# Summary

Bright future is some, you can with no effort get free computing resources for prove of concept. 
