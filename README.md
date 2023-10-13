# AzureInstallCoreSdk.ado
This template can be used to acquire a specific version of the dotNet Core SDK. Also, use this template to change the version of dotNet Core that is used in subsequent tasks.

## Pipeline Requirements

The dotNet Module pipeline requires the following parameters to be defined:

Parameters:

| Name  | Displayname | type | Default | Values | Opional/Required | Comments |
| ------------- | ------------- | :-------------: | :-------------: | :-------------: | :-------------: | ------------- |
| packageType | Package to install | String | 'sdk' | runtime, sdk (SDK (contains runtime)) | Required | Specifies whether to install only the .NET runtime or the SDK |
| useGlobalJson | Use global json | Boolean | false | true / false | Optional | Use when packageType = sdk. Installs all SDKs from global.json files. These files are searched from system.DefaultWorkingDirectory. You can change the search root path by setting the working directory input. |
| installationPath | Path To Install .Net Core  | String | $(Agent.ToolsDirectory)/dotnet |  | Optional | Specifies where the .NET Core SDK/Runtime should be installed. Different paths can have the following impact on .NET's behavior |
| CoreSDKVersion | SDK Version | String | | | Optional | Use when useGlobalJson = false or packageType = runtime. Specifies the version of the .NET Core SDK or runtime to install. The version values for SDK or runtime installations are in the releases.json |
| performMultiLevelLookup | Perform Multi Level Lookup | Boolean | false | true / false | Optional | Configures the behavior of the .NET host process when it searches for a suitable shared framework. Only applicable to Windows based agents. |
| includePreviewVersions | Include Preview Versions | Boolean | false | true / false | Optional | Use when useGlobalJson = false & packageType = runtime. If set to true, includes preview versions when the task searches for latest runtime/SDK versions, such as searching for 2.2.x or 3.1.x. This setting is ignored if you specify an exact version, such as 3.0.100-preview3-010431. |
| vsCodeVersion | Compatible Visual Studio version | String | | | Optional | Specifies a compatible Visual Studio version for a corresponding .NET Core SDK installation. The value must be a complete version number, such as 16.6.4, which contains a major version, a minor version, and a patch number |
| filePath | Working Directory | String | | | Optional | Use when useGlobalJson = true. Specifies the path from where global.json files should be searched when using useGlobalJson. If the value is empty, system.DefaultWorkingDirectory will be considered as the root path. |

These parameters provide multiple use case options for the dotnet templates pipeline, enable/disable flags for the utilization of different templates as per the requirements.


## Use Cases

You can directly call a particular template as per the requirement. for example: 

  ```yaml
  # azure-pipeline.yml
  resources:
  repositories:
    - repository: Template
      type: github
      name: your_username/AzureInstallNugetTool.ado
      ref: <respective branch name>
      endpoint: 'githubServiceConnectioNname'

  steps:
  # passing the parameters
  - template: Install_Core_Sdk.yml@Template
        parameters:
          packageType: ${{ parameters.packageType }}
          useGlobalJson: : ${{ parameters.useGlobalJson }}
          CoreSDKVersion: ${{ parameters.CoreSDKVersion }}
          installationPath: $(Agent.ToolsDirectory)/dotnet

        
  
Make sure to adjust the repository name, branch name, and parameter values according to your project's requirements.

  ```

