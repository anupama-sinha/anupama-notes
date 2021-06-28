## CI/CD Setup in Azure
* Commit code in Feature branch
* Raise Pull Request(PR) to merge to main branch
* Automated PR Review Setup gets invoked
* Once PR approved by Reviewer, code merged to main branch automatically
* Once main brach gets new code, CI Build triggered(Build Code, publish test coverage report, build Docker image, Push the image to Azure Azure Container Registry(ACR))
* Then CD Build gets triggered & Docker image deploye to AKS(Azure Kubernetes Cluster)
