# SamsDotNetSonarCloudAction
A SonarCloud composite action to analyze .NET projects

[![CI/ CD](https://github.com/samsmithnz/SamsDotNetSonarCloudAction/actions/workflows/CI.yml/badge.svg)](https://github.com/samsmithnz/SamsDotNetSonarCloudAction/actions/workflows/CI.yml)
[![Current Release](https://img.shields.io/github/release/samsmithnz/SamsDotNetSonarCloudAction/all.svg)](https://github.com/samsmithnz/SamsDotNetSonarCloudAction/releases)

To use, add this job to your workflow. This is currently marked to only run on the main branch - mostly to avoid the blocking of secrets with dependabot. 

```
  sonarCloud:
    name: Run SonarCloud analysis
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main' 
    steps:
      - name: Run Sonarcloud test
        uses: samsmithnz/SamsDotNetSonarCloudAction@1.1.2
        with:
          projects: 'Sample/ConsoleApp1/ConsoleApp1.csproj'
          dotnet-version: '7.0.x'
          sonarcloud-organization: samsmithnz-github
          sonarcloud-project: samsmithnz_SamsDotNetSonarCloudAction
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```

I've also created an optional parameter to add any required steps before the projects are scanned (e.g. connect to NuGet registry and restore). An example:

```
  sonarCloud:
    name: Run SonarCloud analysis
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main' 
    steps:
      - name: Run Sonarcloud test
        uses: samsmithnz/SamsDotNetSonarCloudAction@1.1.2
        with:
          projects: 'Sample/ConsoleApp1/ConsoleApp1.csproj'
          dotnet-version: '7.0.x'
          sonarcloud-organization: samsmithnz-github
          sonarcloud-project: samsmithnz_SamsDotNetSonarCloudAction
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
          pre-project-steps: 'dotnet nuget add source "https://nuget.pkg.github.com/samsmithnz/index.json" --name "githubfeed" --username "user@email.com" --password "${{ inputs.pre-project-secret }}",dotnet restore Sample/ConsoleApp1/ConsoleApp1.csproj'
          pre-project-secret: ${{ secrets.PackagesReadPAT_Token }}
```
