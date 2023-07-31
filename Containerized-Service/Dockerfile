#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/runtime:6.0 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["Containerized-Service/Containerized-Service.csproj", "Containerized-Service/"]
RUN dotnet restore "Containerized-Service/Containerized-Service.csproj"
COPY . .
WORKDIR "/src/Containerized-Service"
RUN dotnet build "Containerized-Service.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Containerized-Service.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Containerized-Service.dll"]