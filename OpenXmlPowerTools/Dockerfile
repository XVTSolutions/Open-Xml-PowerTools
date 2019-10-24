FROM mcr.microsoft.com/dotnet/core/sdk:3.0 as build

WORKDIR /code

COPY ["OpenXmlPowerTools/OpenXmlPowerTools.csproj", "OpenXmlPowerTools/"]
COPY ["OpenXmlPowerTools.Tests/OpenXmlPowerTools.Tests.csproj", "OpenXmlPowerTools.Tests/"]

# Copy all (non--ignored) files
COPY . .

# Run the Tests
#RUN dotnet test \
	#--logger "trx;LogFileName=/TestResults/OpenXmlPowerTools.Tests.trx" \
    #OpenXmlPowerTools.Tests/OpenXmlPowerTools.Tests.csproj

# Build/Publish with restore (in case the cache isn't complete)
RUN dotnet build "OpenXmlPowerTools/OpenXmlPowerTools.csproj" -c Release -o /dist

# Publish the nuget package
FROM mcr.microsoft.com/dotnet/core/sdk:3.0 AS publish

ARG nugetFeed
ARG nugetApiKey

WORKDIR /dist

COPY --from=build /dist /dist
#COPY --from=build /TestResults /testResults

RUN printf "#!/bin/sh \n dotnet nuget push *.nupkg -s ${nugetFeed} -k ${nugetApiKey}" > /dist/start-nuget-push.sh

ENTRYPOINT ["/bin/sh", "/dist/start-nuget-push.sh"]