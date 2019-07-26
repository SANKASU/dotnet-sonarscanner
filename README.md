# Featured Tags

* `2.2` (Current)
  * `docker pull x430n/dotnet-sonarscanner:2.2`
* `2.1` (LTS)
  * `docker pull x430n/dotnet-sonarscanner:2.1`

# SonarScanner for MSBuild Docker Container

Docker container is based of [Alpine linux](https://www.alpinelinux.org) image to be as small as possible. Beware _Alpine_ linux is not having `bash`
installed by default, but `sh`, if there is need to open container interactively.

Main image is official [OpenJDK](https://hub.docker.com/_/openjdk) `8u212-jre-alpine3.9` since there was issues installing Java to
.NET Core SDK official Docker image (tried Debian, Alpine and Ubuntu).
OpenJRE 8 is minimal requirement to run SonarScanner analysis.

To OpenJDK image is added install of .NET Core Runtime deps merged with .NET Core SDK with set `DOTNET_CLI_TELEMETRY_OPTOUT=true`, just in case.

Added install for [dotnet-sonarscanner](https://github.com/SonarSource/sonar-scanner-msbuild) version `4.6.2` as global dotnet tool and set `PATH` to find it
in `.dotnet/tools` folder.


# Full Tag Listing

Tags        | Dockerfile                   | .NET Core SDK | OS Version
----------- | ---------------------------- | ------------- | -------------
2.2, latest | [Dockerfile](https://github.com/xajler/dotnet-sonarscanner/blob/master/2.2/Dockerfile) | `2.2.401`     | Alpine 3.9
2.1         | [Dockerfile](https://github.com/xajler/dotnet-sonarscanner/blob/master/2.1/Dockerfile) | `2.1.801`     | Alpine 3.9


# Basic usage

Main usage of this Docker Container is in CI (Continuous Integration) environment when CI is not having support for .NET Core version
or there is need to run Build & Test inside of Docker Container.

### Example usage Dockerfile

```dockerfile
# Get sonarscanner for .NET Core SDK 2.2
FROM x430n/dotnet-sonarscanner:2.2 as ci-build

# Get you code repo files including .git
COPY .git ./.git
COPY src ./src
COPY tests  ./tests
COPY myapp.sln ./

# Begin sonarscanner
RUN dotnet-sonarscanner begin /k:my_app

# Do build and run tests
RUN dotnet build && dotnet test

# End sonarscanner and send data
RUN dotnet-sonarscanner end
```
