#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["src/WeatherForecast/WeatherForecast.csproj", "src/WeatherForecast/"]
COPY ["src/WeatherForecast.Contexts/WeatherForecast.Contexts.csproj", "src/WeatherForecast.Contexts/"]
COPY ["src/WeatherForecast.Repositories/WeatherForecast.Repositories.csproj", "src/WeatherForecast.Repositories/"]
COPY ["src/WeatherForecast.Entities/WeatherForecast.Entities.csproj", "src/WeatherForecast.Entities/"]
COPY ["src/WeatherForecast.Interactors/WeatherForecast.Interactors.csproj", "src/WeatherForecast.Interactors/"]
RUN dotnet restore "src/WeatherForecast/WeatherForecast.csproj"
COPY . .
WORKDIR "/src/src/WeatherForecast"
RUN dotnet build "WeatherForecast.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WeatherForecast.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WeatherForecast.dll"]