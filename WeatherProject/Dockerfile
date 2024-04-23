#FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
FROM registry.access.redhat.com/ubi8/dotnet-80-runtime:8.0-6 AS base
USER root
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

#FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
FROM registry.access.redhat.com/ubi8/dotnet-80:8.0-6 AS build
USER root
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["WeatherProject/WeatherProject.csproj", "WeatherProject/"]

#RUN dotnet restore "WeatherProject/WeatherProject.csproj"
COPY . .
WORKDIR "/src/WeatherProject"
RUN #dotnet publish "WeatherProject.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

RUN dotnet build "./WeatherProject.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
USER root
RUN dotnet publish "WeatherProject.csproj" -c $BUILD_CONFIGURATION -o /app/publish

FROM base AS final
USER root
WORKDIR /app
COPY --from=publish /app/publish .
USER default
ENTRYPOINT ["dotnet", "WeatherProject.dll"]
