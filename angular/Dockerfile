#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0-buster-slim AS build
WORKDIR /src

# BEGIN MODIFICATION - Node is needed for development (but not production)
RUN apt-get clean && apt-get update
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash -
RUN apt-get install --assume-yes nodejs
# END MODIFICATION

COPY ["angular/angular.csproj", "angular/"]
RUN dotnet restore "angular/angular.csproj"
COPY . .
WORKDIR "/src/angular"
RUN dotnet build "angular.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "angular.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "angular.dll"]