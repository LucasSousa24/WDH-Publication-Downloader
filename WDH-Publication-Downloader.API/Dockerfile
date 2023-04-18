#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["WDH-Publication-Downloader.API/WDH-Publication-Downloader.API.csproj", "WDH-Publication-Downloader.API/"]
RUN dotnet restore "WDH-Publication-Downloader.API/WDH-Publication-Downloader.API.csproj"
COPY . .
WORKDIR "/src/WDH-Publication-Downloader.API"
RUN dotnet build "WDH-Publication-Downloader.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WDH-Publication-Downloader.API.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WDH-Publication-Downloader.API.dll"]