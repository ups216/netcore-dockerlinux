FROM microsoft/aspnetcore:2.0 AS base
WORKDIR /app
EXPOSE 80

FROM microsoft/aspnetcore-build:2.0 AS build
WORKDIR /src
COPY netcore-dockerlinux/netcore-dockerlinux.csproj netcore-dockerlinux/
RUN dotnet restore netcore-dockerlinux/netcore-dockerlinux.csproj
COPY . .
WORKDIR /src/netcore-dockerlinux
RUN dotnet build netcore-dockerlinux.csproj -c Release -o /app

FROM build AS publish
RUN dotnet publish netcore-dockerlinux.csproj -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "netcore-dockerlinux.dll"]
