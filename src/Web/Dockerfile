# RUN ALL CONTAINERS FROM ROOT (folder with .sln file):
# docker-compose build
# docker-compose up
#
# RUN JUST THIS CONTAINER FROM ROOT (folder with .sln file):
# docker build --pull -t web -f src/Web/Dockerfile .
#
# RUN COMMAND
#  docker run --name eshopweb --rm -it -p 5106:5106 web
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /app

COPY *.sln .
COPY *.csproj .
COPY . .
WORKDIR /app/src/Web
RUN dotnet restore

RUN dotnet publish -c Release -o out

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS runtime
WORKDIR /app
# COPY contrast /app/contrast
 COPY --from=build /app/src/Web/out ./

# Optional: Set this here if not setting it from docker-compose.yml
ENV ASPNETCORE_ENVIRONMENT Docker
ENV ASPNETCORE_URLS http://+:80
# VOLUME ~/.aspnet/https:/root/.aspnet/https:ro
# VOLUME ~/.microsoft/usersecrets:/root/.microsoft/usersecrets:ro

# COPY --from=contrast/agent-dotnet-core:latest /contrast /contrast
# COPY ./contrast /contrast
# VOLUME C:/contrast/
ENV CORECLR_PROFILER_PATH_64=/contrast/runtimes/linux-x64/native/ContrastProfiler.so 
ENV CORECLR_ENABLE_PROFILING=1 
ENV CORECLR_PROFILER={8B2CE134-0948-48CA-A4B2-80DDAD9F5791} 
ENV CONTRAST__API__URL=https://eval.contrastsecurity.com/Contrast 
ENV CONTRAST__API__API_KEY=ZhSrghj5Yot67rMA43ROtOJm1Ufn1wJD 
ENV CONTRAST__API__SERVICE_KEY=Y87MVBN0Q87W3QL0 
ENV CONTRAST__API__USER_NAME=agent_ad7c8156-e1b3-4570-aea3-c1f7d1226626@OnetPartner 
ENV CONTRAST__APPLICATION__NAME=eShoppeOnWeb 
ENV CONTRAST__SERVER__NAME=DockerDotNETServer

ENTRYPOINT ["dotnet", "Web.dll"]
