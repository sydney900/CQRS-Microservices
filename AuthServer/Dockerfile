FROM microsoft/dotnet:2.2-aspnetcore-runtime AS base
WORKDIR /app
EXPOSE 80

FROM microsoft/dotnet:2.2-sdk AS build
WORKDIR /src/WebApiCommon
COPY WebApiCommon/WebApiCommon.csproj .
COPY WebApiCommon/. .
RUN dotnet restore -nowarn:msb3202,nu1503
RUN dotnet build -c Release

WORKDIR /src
COPY ["AuthServer/AuthServer.csproj", "AuthServer/"]
RUN dotnet restore "AuthServer/AuthServer.csproj"
COPY AuthServer/. AuthServer/.
WORKDIR "/src/AuthServer"
RUN dotnet build "AuthServer.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "AuthServer.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "AuthServer.dll", "/seed"]