# qt-cloud-build-test

Project sample to test Qt build on cloud service\
Tests implemented with Qt 5.10.0 and MinGW 5.3.0

### Tested services:
* [appveyor.yml](appveyor.yml) : AppVeyor (https://www.appveyor.com/)
* [buildspec.yml](buildspec.yml) : AWS CodeBuild (https://aws.amazon.com/codebuild/)
  * Only works with a custom image with Qt installation (https://aws.amazon.com/pt/blogs/devops/extending-aws-codebuild-with-custom-build-environments/)
* [azure-pipelines.yml](azure-pipelines.yml) : Azure Pipelines (https://azure.microsoft.com/en-us/services/devops/pipelines/)

#### Dockerfile example for AWS CodeBuild image:
```Dockerfile
FROM mcr.microsoft.com/windows/servercore:ltsc2016
LABEL Description="Windows Server Core development environment for Qt 5.10.0"

# Install Qt 5.10.0 (copy)
COPY install_qt-5.10.0.qs C:\\install_qt-5.10.0.qs
COPY qt-opensource-windows-x86-5.10.0.exe C:\\qt-5.10.0.exe
RUN dir "C:"
RUN powershell -NoProfile -ExecutionPolicy Bypass -Command \
    $ErrorActionPreference = 'Stop'; \
    $Env:QT_INSTALL_DIR = 'C:\\Qt' ; \
    Start-Process C:\qt-5.10.0.exe -ArgumentList '--verbose --script C:\\install_qt-5.10.0.qs' -NoNewWindow -Wait ; \
    Remove-Item C:\qt-5.10.0.exe -Force ; \
    Remove-Item C:\install_qt-5.10.0.qs -Force

ENV QTDIR C:\\Qt\\5.10.0\\mingw53_32
ENV QTTOOLS C:\\Qt\\Tools\\mingw530_32
RUN dir "%QTDIR%" && dir "%QTDIR%\bin"
RUN dir "%QTTOOLS%" && dir "%QTTOOLS%\bin"
```

#### Silent intaller Qt script example (install_qt-5.10.0.qs) used by Dockerfile:
```Javascript
function Controller() {
	gui.setSilent(true);

    installer.autoRejectMessageBoxes();
    installer.setMessageBoxAutomaticAnswer("OverwriteTargetDirectory", QMessageBox.Yes);
    installer.setMessageBoxAutomaticAnswer("installationErrorWithRetry", QMessageBox.Ignore);
    installer.installationFinished.connect(function() {
        console.log("install finished");
        gui.clickButton(buttons.NextButton);
    })
}

Controller.prototype.WelcomePageCallback = function() {
    console.log("Welcome");
    gui.clickButton(buttons.NextButton, 3000);
}

Controller.prototype.CredentialsPageCallback = function() {
    console.log("CredentialsPageCallback");
    gui.clickButton(buttons.NextButton);
}

Controller.prototype.IntroductionPageCallback = function() {
    console.log("IntroductionPageCallback");
    gui.clickButton(buttons.NextButton);
}

Controller.prototype.TargetDirectoryPageCallback = function()
{
    console.log("TargetDirectoryPageCallback");
    var widget = gui.currentPageWidget();
    gui.currentPageWidget().TargetDirectoryLineEdit.setText("C:\\Qt");
    gui.clickButton(buttons.NextButton, 3000);
}

Controller.prototype.ComponentSelectionPageCallback = function() {
    var widget = gui.currentPageWidget();

    widget.deselectAll();
    widget.selectComponent("qt.qt5.5100.win32_mingw53");
	widget.selectComponent("qt.tools.win32_mingw530");

    gui.clickButton(buttons.NextButton);
}

Controller.prototype.LicenseAgreementPageCallback = function() {
    console.log("LicenseAgreementPageCallback");
    gui.currentPageWidget().AcceptLicenseRadioButton.setChecked(true);
    gui.clickButton(buttons.NextButton);
}

Controller.prototype.StartMenuDirectoryPageCallback = function() {
    console.log("StartMenuDirectoryPageCallback");
    gui.clickButton(buttons.NextButton);
}

Controller.prototype.ReadyForInstallationPageCallback = function()
{
    console.log("ReadyForInstallationPageCallback");
    gui.clickButton(buttons.NextButton);
}

Controller.prototype.FinishedPageCallback = function() {
    console.log("FinishedPageCallback");
    var lanchQtCreatorCheckBoxForm = gui.currentPageWidget().LaunchQtCreatorCheckBoxForm;
    if(lanchQtCreatorCheckBoxForm)
    {
        lanchQtCreatorCheckBoxForm.launchQtCreatorCheckBox.setChecked(false);
    }
    gui.clickButton(buttons.FinishButton);
    console.log("Press FinishButton");
}
```
