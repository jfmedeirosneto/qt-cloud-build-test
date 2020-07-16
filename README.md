# qt-cloud-build-test

Project sample to test Qt build on cloud service\
Tests maked with Qt 5.10.0 and MinGW 5.3.0

### Tested services:

* [appveyor.yml](appveyor.yml) : AppVeyor (https://www.appveyor.com/)
* [buildspec.yml](buildspec.yml) : AWS CodeBuild (https://aws.amazon.com/codebuild/)
  * Only works with a custom image with Qt installation (https://aws.amazon.com/pt/blogs/devops/extending-aws-codebuild-with-custom-build-environments/)
* [azure-pipelines.yml](azure-pipelines.yml) : Azure Pipelines (https://azure.microsoft.com/en-us/services/devops/pipelines/)
