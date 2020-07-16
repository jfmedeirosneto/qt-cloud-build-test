# qt-cloud-build-test

Project to test qt build on cloud

* [appveyor.yml](appveyor.yml) : test on AppVeyor (https://www.appveyor.com/)
* [buildspec.yml](buildspec.yml) : test on AWS CodeBuild (https://aws.amazon.com/codebuild/)
  * Only works with a custom image with Qt installation (https://aws.amazon.com/pt/blogs/devops/extending-aws-codebuild-with-custom-build-environments/)
* [azure-pipelines.yml](azure-pipelines.yml) : test on Azure Pipelines (https://azure.microsoft.com/en-us/services/devops/pipelines/)
